### simple app with webpack-dev-server
*this is for a simple page that does not use Express or other server*
```json
const path = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
	context: path.resolve(__dirname, 'public'),
  entry: './index.js',
  devtool: 'cheap-eval-source-map;,
  output: {
    path: path.join(__dirname, '/public'),
	  filename: 'bundle.js'
  },
  plugins: [
		// detects changes to html files
		new HtmlWebpackPlugin({
			template: path.join(__dirname, '/public/index.html')
		})
	],
	devServer: {
		host: '0.0.0.0',
		inline: true,
		port: 8080
	}
}
```
