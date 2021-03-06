#!/usr/bin/env rake

require_relative './app.rb'

namespace :unicorn do

  desc "Start unicorn"
  task(:start) {
    config = root + "/config/unicorn.rb"
    sh "bundle exec unicorn --daemonize --config-file \"#{config}\""
    p "Unicorn started"
  }

  desc "Stop unicorn"
  task(:stop) {
    unicorn_signal :QUIT
    p "Unicorn stopped"
  }

  desc "Restart unicorn with USR2"
  task(:restart) {
    unicorn_signal :USR2
    p "Unicorn restarted"
  }

  desc "Increment number of worker processes"
  task(:increment) {
    unicorn_signal :TTIN
    p "Started one more Unicorn worker"
  }

  desc "Decrement number of worker processes"
  task(:decrement) {
    unicorn_signal :TTOU
    p "Stopped one Unicorn worker"
  }

  desc "Unicorn pstree (depends on pstree command)"
  task(:pstree) do
    sh "pstree '#{unicorn_pid}'"
  end

  ##
  # Helpers

  def unicorn_signal signal
    Process.kill signal, unicorn_pid
  end

  def unicorn_pid
    begin
      File.read(root + "/tmp/unicorn.pid").to_i
    rescue Errno::ENOENT
      raise "Unicorn doesn't seem to be running"
    end
  end

  def root
    APP_ROOT
  end

end