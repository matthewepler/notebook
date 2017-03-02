# Notes from Adam Goldman Videos
Convention is to have /src and /dist in the base of the project. 
/dist should not be tracked by version control
/node_modules should not be tracked by version control

In webpack, instead of excluding directories (i.e. `exclude: node_modules`), instead explicity declare the directories you want to include (i.e. `include: /src/`)

when using 'resolve' to set paths, separate the directory levels, i.e. `__dirname, 'src', 'index.js') instead of `__dirname, 'src/index.js')`

convention with webpack is to use 'context,' as it is easily readable by others to know where the base of the app is. 

query selectors, that appear in side a function that will get called for than twice should be pu t in global scope. 
