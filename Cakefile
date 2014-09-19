fs     = require 'fs'
{exec} = require 'child_process'

callback = (callback) ->
  (err, stdout, stderr) ->
    throw err if err
    console.log stdout if stdout
    console.error stderr if stderr
    callback() if callback

task 'build', 'Build project', ->
  exec "cake build:octicons", callback ->
    exec "cake build:less", callback ->
  exec "cake build:coffee", callback ->

task 'build:coffee', 'Build scripts', ->
  exec "coffee --compile assets/js/", callback ->
    exec "uglifyjs --mangle --output buttons.js assets/js/buttons.js", callback ->

task 'build:less', 'Build stylesheets', ->
  for file in fs.readdirSync 'assets/css/' when file.match /\.less$/
    exec "lessc assets/css/#{file} assets/css/#{file.replace /\.less$/, '.css'}", callback ->

task 'build:octicons', 'Build octicons', ->
  exec "phantomjs src/octicons/base.coffee assets/css/octicons/base.less", callback ->
  exec "phantomjs src/octicons/lt-ie8.coffee assets/css/octicons/lt-ie8.css", callback ->
