{spawn, exec} = require 'child_process'
log = console.log

task 'build', ->
  run 'coffee -o lib -c src/*.coffee'

task 'test', ->
  run 'cd test; coffee templateTests.coffee'

task 'clean', ->
  run 'npm remove -g coffin'

task 'install', ->
  run 'npm install -g .'

task 'all', ->
  run 'cake clean; cake test; cake build; cake install'

run = (args...) ->
  for a in args
    switch typeof a
      when 'string' then command = a
      when 'object'
        if a instanceof Array then params = a
        else options = a
      when 'function' then callback = a

  command += ' ' + params.join ' ' if params?
  cmd = spawn '/bin/sh', ['-c', command], options
  cmd.stdout.on 'data', (data) -> process.stdout.write data
  cmd.stderr.on 'data', (data) -> process.stderr.write data
  process.on 'SIGHUP', -> cmd.kill()
  cmd.on 'exit', (code) -> callback() if callback? and code is 0
