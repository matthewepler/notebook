### simple app with webpack-dev-server
*this is for a simple page that does not use Express or other server*
```json
const { resolve } = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: resolve(__dirname, 'public/index.js'),
  devtool: 'cheap-eval-source-map;,
  output: {
    path: path.join(__dirname, '/public'),
	  filename: 'bundle.js'
  },
  plugins: [
		// detects changes to html files
		new HtmlWebpackPlugin({
			template: path.join(__dirname, 'public/index.html')
		}),
		new webpack.NamedModulesPlugin()
	],
	devServer: {
		contentBase: resolve(__dirname, 'public'),
		host: '0.0.0.0',
		inline: true,
		port: 8080,
		publicPath: '/'
	}
}
```
