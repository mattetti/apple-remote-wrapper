Remote Control Wrapper
----------------------

You can get the original version of this code from http://martinkahr.com/source-code/

Remote Control Wrapper is an Objective-C wrapper for the Apple remote control.
It was modified to be Obj-C 2 compatible and compile as a framework to be used with MacRuby.
Note that to use this framework with MacRuby you need to use a BridgeSupport file (generated for you in the repo).
A compiled version of the framework is available in the download section.

Here is a quick implementation example using MacRuby and HotCocoa:


require 'hotcocoa'
framework File.join(File.dirname(__FILE__), 'vendor', 'AppleRemote.framework')

class Application

  include HotCocoa
  
  def start
    application :name => "AppRemote" do |app|
      app.delegate = self
      window :frame => [100, 100, 500, 500], :title => "Appremote" do |win|
        win << label(:text => "Application Remote", :layout => {:start => false})
        win.will_close { exit }
      end
      @remoteControl = AppleRemote.alloc.initWithDelegate(self)
      @remoteControl.startListening(self)
    end
  end
  
  # Callback triggered when the remote is being used.
  #
  # === Parameters
  # button<Fixnum>:: button id
  # pressed<Boolean>:: is the button pressed or released
  # remote<AppleRemote>::
  def sendRemoteButtonEvent(button, pressedDown:pressed, remoteControl:remote)
    p "button pressed #{button} #{pressed ? 'clicked' : 'released'}"
  end
end