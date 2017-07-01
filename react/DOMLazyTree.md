#### DOMLazyTree.js源码

```js
'use strict';

var DOMNamespaces = require('DOMNamespaces');
var setInnerHTML = require('setInnerHTML');
var {DOCUMENT_FRAGMENT_NODE, ELEMENT_NODE} = require('HTMLNodeType');
var createMicrosoftUnsafeLocalFunction = require('createMicrosoftUnsafeLocalFunction');
var setTextContent = require('setTextContent');

/**
 * IE (8-11)和Edge,添加节点的时候,添加no Children的节点的速度显著比添加整棵树快，其他浏览器还是
 * 照样appendChild添加
 */
// 是否是IE (8-11)和Edge浏览器
var enableLazy =
  (typeof document !== 'undefined' &&
    typeof document.documentMode === 'number') ||
  (typeof navigator !== 'undefined' &&
    typeof navigator.userAgent === 'string' &&
    /\bEdge\/\d/.test(navigator.userAgent));

function insertTreeChildren(tree) {
  if (!enableLazy) {
    return;
  }
  var node = tree.node;
  var children = tree.children;
  if (children.length) {
    for (var i = 0; i < children.length; i++) {
      // 递归增加
      insertTreeBefore(node, children[i], null);
    }
  } else if (tree.html != null) {
    setInnerHTML(node, tree.html);
  } else if (tree.text != null) {
    setTextContent(node, tree.text);
  }
}

var insertTreeBefore = createMicrosoftUnsafeLocalFunction(function(
  parentNode,
  tree,
  referenceNode,
) {
  // DocumentFragments aren't actually part of the DOM after insertion so
  // appending children won't update the DOM. We need to ensure the fragment
  // is properly populated first, breaking out of our lazy approach for just
  // this level. Also, some <object> plugins (like Flash Player) will read
  // <param> nodes immediately upon insertion into the DOM, so <object>
  // must also be populated prior to insertion into the DOM.
  // DocumentFragments与Object节点特殊处理，一次性添加
  if (
    tree.node.nodeType === DOCUMENT_FRAGMENT_NODE ||
    (tree.node.nodeType === ELEMENT_NODE &&
      tree.node.nodeName.toLowerCase() === 'object' &&
      (tree.node.namespaceURI == null ||
        tree.node.namespaceURI === DOMNamespaces.html))
  ) {
    // 特殊情况，先曾加节点，再直接整棵树增加
    insertTreeChildren(tree);
    parentNode.insertBefore(tree.node, referenceNode);
  } else {
    // 递归增加noChildren节点
    parentNode.insertBefore(tree.node, referenceNode);
    insertTreeChildren(tree);
  }
});

function replaceChildWithTree(oldNode, newTree) {
  oldNode.parentNode.replaceChild(newTree.node, oldNode);
  insertTreeChildren(newTree);
}

function queueChild(parentTree, childTree) {
  if (enableLazy) {
    parentTree.children.push(childTree);
  } else {
    parentTree.node.appendChild(childTree.node);
  }
}

function queueHTML(tree, html) {
  if (enableLazy) {
    tree.html = html;
  } else {
    setInnerHTML(tree.node, html);
  }
}

function queueText(tree, text) {
  if (enableLazy) {
    tree.text = text;
  } else {
    setTextContent(tree.node, text);
  }
}

function toString() {
  return this.node.nodeName;
}

function DOMLazyTree(node) {
  return {
    node: node,
    children: [],
    html: null,
    text: null,
    toString,
  };
}

DOMLazyTree.insertTreeBefore = insertTreeBefore;
DOMLazyTree.replaceChildWithTree = replaceChildWithTree;
DOMLazyTree.queueChild = queueChild;
DOMLazyTree.queueHTML = queueHTML;
DOMLazyTree.queueText = queueText;

module.exports = DOMLazyTree;

```