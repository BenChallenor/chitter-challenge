require 'pg'

if ENV['RACK_ENV'] != 'production'
  require 'rspec/core/rake_task'

  RSpec::Core::RakeTask.new :spec

  task default: [:spec]
end

task :setup do
  p "Creating databases..."

  ['chitter', 'chitter_test'].each do |database|
    connection = PG.connect
    connection.exec("DROP DATABASE #{ database };")
    connection.exec("CREATE DATABASE #{ database };")
    connection = PG.connect(dbname: database)
    connection.exec("CREATE TABLE peeps(id SERIAL PRIMARY KEY, post VARCHAR(240), time TIMESTAMP);")
  end
end

task :test_database_setup do
  p "Cleaning database..."

  connection = PG.connect(dbname: 'chitter_test')

  # Clear the database
  connection.exec("TRUNCATE peeps;")

  connection.exec("INSERT INTO peeps VALUES(1, 'This is a test peep', '#{Time.now}');")
  connection.exec("INSERT INTO peeps VALUES(2, 'This is another test peep', '#{Time.now}');")
end
