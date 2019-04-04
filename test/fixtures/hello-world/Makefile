default: hello

run:
	./hello -c cppcms.js

hello: hello.cpp
	c++ hello.cpp -o hello -Lcppcms/lib -Icppcms/include -lcppcms -lbooster -lz

clean:
	rm -f hello

####################
# for local development:
#   make devlibs && make hello && LD_LIBRARY_PATH=cppcms/lib make run

devclean:
	rm -rf cppcms
	rm -f cppcms-1.2.1-linux-x64.tgz

devlibs: cppcms-1.2.1-linux-x64.tgz
	mkdir -p cppcms
	tar -m --directory cppcms -zxf cppcms-1.2.1-linux-x64.tgz

cppcms-1.2.1-linux-x64.tgz:
	wget -c 'https://github.com/flavorjones/cppcms-buildpack-test/blob/master/vendor/cflinuxfs2/cppcms-1.2.1-linux-x64.tgz?raw=true' -O cppcms-1.2.1-linux-x64.tgz
