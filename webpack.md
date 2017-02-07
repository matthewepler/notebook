### simple app with webpack-dev-server
*this is for a simple page that does not use Express or other server*
```json
const { resolve } = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: resolve(__dirname, 'public/index.js'),
  devtool: 'cheap-eval-source-map',
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

**Environment Variables (and Docker)**
Webpack does not inherit system environment variables because it is separate from Node. There are varying ways to get them in, but this is the only one I could get to work with Docker.

My goal was to set an environment variable in the docker-compose.yml file and get it all the way to the javascript being bundled by webpack.

In docker-compose.yml
```yaml
version: '2'

services:
  client1:
		...
		environment:
			- NAME=client1
		...
```

In webpack.config.js, name the environment variable you want to make accessible inside your bundled javascript
```javascript
const webpack = require('webpack')

module.exports = {
	entry: ...
	...
	plugins: [
		...
		new webpack.EnvironmentPlugin(['NAME'])
	]
}
```

In package.json, link the existing environment variable (created in the compose file) to a variable that EnvironmentPlugin can see.
```json
{
	"name": "mqtt_assignment",
	...
	"scripts": "NAME=${NAME:=noname} webpack-dev-server"
}
```
This sets the contents of the existing env variable 'NAME' (${NAME}) to a variable that is passed to the webpack compiler (NAME=). If there is no value for ${NAME}, then it will default to 'noname.'
