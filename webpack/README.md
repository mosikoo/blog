```
  config.externals = {
    '@ali/cg-react': 'var CGReact',
    'react': 'var React',
    'react-dom': 'var ReactDOM'
  };
```

```
    config.resolve.alias = {
      react: path.join(__dirname, 'node_modules', 'react'),
      'react-dom': path.join(__dirname, 'node_modules', 'react-dom')
    };

    if (process.env.NODE_ENV === 'develop') {
      // debug模式
      config.devtool = 'eval-source-map';
    }

```