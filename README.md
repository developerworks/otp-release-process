Otp release process
===================

This project is a sample application of explaining the [OTP Release system](http://erlang.org/doc/design_principles/release_handling.html#11)

Validation application
----------------------

    erl -pa ./ebin -init_debug -boot start_sasl
	application:start(tiny).
	Eshell V5.9.1  (abort with ^G)
	1> application:start(tiny).
	=PROGRESS REPORT==== 24-Jul-2012::10:38:56 ===
	         application: tiny
	          started_at: nonode@nohost
	ok
	2> tiny 1.0 running...
	2> tiny 1.0 running...
	2> tiny 1.0 running...
	2> tiny 1.0 running...
	
	
Release application with otp release system
-------------------------------------------

Create tiny-1.0.rel release file

	{release, 
		{"tiny app release", "1.0"}, 
		{erts, "5.6.5"},
		[
			{kernel, "2.12.5"},
		    {stdlib, "1.15.5"},
		    {sasl, "2.1.5.4"},
		    {tiny_app, "1.0"}
		]
	}.

Packaging
---------

	erl -pa ./ebin ../ -boot start_sasl
	
	=PROGRESS REPORT==== 24-Jul-2012::10:57:10 ===
	          supervisor: {local,sasl_safe_sup}
	             started: [{pid,<0.33.0>},
	                       {name,alarm_handler},
	                       {mfargs,{alarm_handler,start_link,[]}},
	                       {restart_type,permanent},
	                       {shutdown,2000},
	                       {child_type,worker}]
	
	=PROGRESS REPORT==== 24-Jul-2012::10:57:11 ===
	          supervisor: {local,sasl_safe_sup}
	             started: [{pid,<0.34.0>},
	                       {name,overload},
	                       {mfargs,{overload,start_link,[]}},
	                       {restart_type,permanent},
	                       {shutdown,2000},
	                       {child_type,worker}]
	Running Erlang
	
	=PROGRESS REPORT==== 24-Jul-2012::10:57:11 ===
	          supervisor: {local,sasl_sup}
	             started: [{pid,<0.32.0>},
	                       {name,sasl_safe_sup},
	                       {mfargs,
	                           {supervisor,start_link,
	                               [{local,sasl_safe_sup},sasl,safe]}},
	                       {restart_type,permanent},
	                       {shutdown,infinity},
	                       {child_type,supervisor}]
	
	=PROGRESS REPORT==== 24-Jul-2012::10:57:11 ===
	          supervisor: {local,sasl_sup}
	             started: [{pid,<0.35.0>},
	                       {name,release_handler},
	                       {mfargs,{release_handler,start_link,[]}},
	                       {restart_type,permanent},
	                       {shutdown,2000},
	                       {child_type,worker}]
	Eshell V5.9.1  (abort with ^G)
	
	=PROGRESS REPORT==== 24-Jul-2012::10:57:11 ===
	         application: sasl
	          started_at: nonode@nohost
	1> target_system:create("tiny-1.0").
	Reading file: "tiny-1.0.rel" ...
	Creating file: "./plain.rel" from "tiny-1.0.rel" ...
	Making "./plain.script" and "./plain.boot" files ...
	*WARNING* : Missing application sasl. Can not upgrade with this release
	Making "tiny-1.0.script" and "tiny-1.0.boot" files ...
	Creating tar file "./tiny-1.0.tar.gz" ...
	Creating directory "./tmp" ...
	Extracting "./tiny-1.0.tar.gz" into directory "./tmp" ...
	Deleting "erl" and "start" in directory "./tmp/erts-5.9.1/bin" ...
	Creating temporary directory "./tmp/bin" ...
	Copying file "./plain.boot" to "./tmp/bin/start.boot" ...
	Copying files "epmd", "run_erl" and "to_erl" from
	"./tmp/erts-5.9.1/bin" to "./tmp/bin" ...
	Creating "./tmp/releases/start_erl.data" ...
	Recreating tar file "./tiny-1.0.tar.gz" from contents in directory "./tmp" ...
	Removing directory "./tmp" ...
	ok
	
References
----------

* [How to create an Erlang First Target System](http://streamhacker.com/2009/07/02/how-to-create-an-erlang-first-target-system/)
* [erlang otp 应用发布指南(一) tiny-1.0非真正OTP](http://erlangdisplay.iteye.com/blog/342819)
* [使用target_system进行erlang应用的发行 ](http://blog.csdn.net/sw2wolf/article/details/6797716)
