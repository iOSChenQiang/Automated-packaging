# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

default_platform(:ios)

platform :ios do

  before_all do
# enter your specific password
    ENV["FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD"] = "specific password"
    cocoapods(use_bundle_exec: FALSE)
    puts "所有任务执行开始了"

  end

  desc "上线 App Store"
  lane :release do | options |

   	#setup_version_build(options)

    increment_build_number(xcodeproj: ENV['Xcodeproj'])
    build_app(workspace: ENV['Workspace'], scheme: ENV['Scheme'])
    upload_to_app_store(force: true)
  end

  desc "上线testflight"
  lane :beta do | options |
    #increment_build_number(xcodeproj: ENV['Xcodeproj'])
    #build_app(workspace: ENV['Workspace'], scheme: ENV['Scheme'])
    #upload_to_testflight
  end

  desc "上线蒲公英"
  lane :adhoc_pgy do |options|

   increment_build_number(xcodeproj:ENV['Xcodeproj'])
  
   currentTime = Time.new.strftime("%Y-%m-%d-%H-%M-")
   logDirectory = "#{currentTime}"

    build_app(
	workspace: ENV['Workspace'], 
	scheme: ENV['Scheme'],
 	silent: true,
 	clean: true,
	output_directory: ENV['Pgy_Output_Path'],
	output_name:"#{currentTime}#{ENV['ipa']}",
	
# xcodebuild ios 9
   	export_xcargs: "-allowProvisioningUpdates",
	export_method:"development")

   pgyer(api_key: ENV['api_key'], 
	user_key: ENV['user_key'])

  end


  desc "上线firim"
  lane :adhoc_firim do |options|

   setup_version_build(options)

   currentTime = Time.new.strftime("%Y-%m-%d-%H-%M-")
   logDirectory = "#{currentTime}"
	
   build_app(
	workspace: ENV['Workspace'], 
	scheme: ENV['Scheme'],
 	silent: true,
 	clean: true,
	output_directory: ENV['Firim_Output_Path'],
	output_name: "#{currentTime}#{ENV['ipa']}",
   	export_xcargs: "-allowProvisioningUpdates",
	export_method:"ad-hoc")

  firim(firim_api_token:ENV['firim_api_token'])

  end

# 函数
  desc "版本处理"
  def setup_version_build(options)
    increment_build_number(
	   build_number:options[:build]
	)
    increment_version_number(
	   version_number:options[:version]
	)
 	
  end


  after_all do |lane, options|

    puts"结束了"
  end

  error do |lane, exception, options|
    if options[:debug]
     puts "Hi :)"
    end
  end

end
