# FreeBSD ��װ shadowsocks-server

> �ο�[�����FreeBSD�а�װShadowsocks��ʹ��PAC��ѧ����](https://www.tomczhen.com/2015/12/08/howto-install-shadowsocks-in-freebsd-and-use-pac/)

1. ��װ
		
	FreeBSDʹ�ð�������pkg
		
		pkg install shadowsocks-libev
		
	�˲����ڲ���ʱdpkg��ʾ�����������������dpk��ʹ���򱨴�
	
		/usr/local/lib/libpkg.so.4: Undefined symbol "openat"
	
	[������ѯ����������������ͬ������](https://yq.aliyun.com/ask/46495)����һ¥������ԭ����ʱ�����������ʱ��ʹ��pkg-static�����滻pkg
	
		pkg-static install shadowsocks-libev
		
2. ����

> �ο��ٷ�ʾ��[https://github.com/madeye/shadowsocks-libev/blob/master/debian/config.json](https://github.com/madeye/shadowsocks-libev/blob/master/debian/config.json)

ͨ�� ss-server -c ������ָ�������ļ�
	
ss-server help
	
		shadowsocks-libev 1.6.4

		maintained by Max Lv <max.c.lv@gmail.com> and Linus Yang <laokongzi@gmail.com>

		usage:

		ss-[local|redir|server|tunnel]

			  -s <server_host>           host name or ip address of your remote server
			  -p <server_port>           port number of your remote server
			  -l <local_port>            port number of your local server
			  -k <password>              password of your remote server


			  [-m <encrypt_method>]      encrypt method: table, rc4, rc4-md5,
										 aes-128-cfb, aes-192-cfb, aes-256-cfb,
										 bf-cfb, camellia-128-cfb, camellia-192-cfb,
										 camellia-256-cfb, cast5-cfb, des-cfb, idea-cfb,
										 rc2-cfb, seed-cfb, salsa20 and chacha20
			  [-f <pid_file>]            file to store the pid
			  [-t <timeout>]             socket timeout in seconds
			  [-c <config_file>]         config file in json


			  [-i <interface>]           network interface to bind,
										 not available in redir mode
			  [-b <local_address>]       local address to bind,
										 not available in server mode
			  [-u]                       enable udprelay mode
										 not available in redir mode
			  [-L <addr>:<port>]         setup a local port forwarding tunnel,
										 only available in tunnel mode
			  [-v]                       verbose mode


			  [--fast-open]              enable TCP fast open,
										 only available on Linux kernel > 3.7.0
			  [--acl <acl_file>]         config file of ACL (Access Control List)


��������ʾ��

	{
		"server":"0.0.0.0",
		"server_port":11111,
		"local_address":"127.0.0.1",
		"local_port":1080,
		"password":"password",
		"timeout":300,
		"method":"aes-256-cfb",
		"fast_open":false
	}

3. ���÷���
> �ο�[https://github.com/shadowsocks/shadowsocks-libev/issues/19](https://github.com/shadowsocks/shadowsocks-libev/issues/19)

	1. ����������ݵ� `/etc/rc.conf`
	
			shadowsocks_libev_enable="YES"
			shadowsocks_libev_flags="-c �����ļ�λ��"
			
	2. ִ��������������
	
			service shadowsocks_libev start
			
ʵ���ڲ��������з��ַ�������֮����˻Ὣshadowsocks_libev_flags������ӵ������������Զ�׷�Ӳ������²���

		-f /var/run/shadowsocks-libev.pid -c /usr/local/etc/shadowsocks-libev/config.json
		
����-fΪָ��������pid�洢λ�ã�-c Ϊָ�������ļ�

���Ǹո��Ѿ���shadowssocks_libev_flags��ָ����-c��������ss-server����������»�ʶ������ָ���Ĳ������ʵ����˲��ܴﵽԤ�ڵ�Ч��

���ķ�̫�ྫ����ʱ�䣬����ȥֱ���޸�`/usr/local/etc/shadowsocks-libev/config.json`Ϊ�Լ���Ҫ�Ĳ�������ע��`/etc/rc.conf`�е��� shadowsocks_libev_flags="-c �����ļ�λ��"
			
			
			
			
			
			
			
			