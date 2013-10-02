3. Create a configuration that will allow you to delegate the reverse(PTR) records to another server for IP range smaller then /24 for example /26. Show the named.conf and the zone file.


Общи условия:
Имаме мрежа 78.108.248.0/24
Решаваме да разцепим мрежата на 26-ия бит. 192.168.1.1-192 остава при делегиращия сървър и той ще си описва PTR-ите. 192.168.1.193-255 ще се описва от делегирания сървър.
Делегиращия dns сървър ще бъде ns1.net.com
Делегирания - ns1.subnet.com

При делегиращия сървър в named.conf
 
============================================
zone "248.108.78.in-addr.arpa" {
	type master;
	file "/etc/bind/78.108.248.1_192.reverse.db"
	notify no;
};

========================================================
Зоне файл на делегиращия сървър /etc/bind/78.108.248.1_192.reverse.db. Важен е последния ред делегиращ dns сървъръра ns1.subnet.net
==========================================================

$TTL 86400
@	IN SOA ns1.net.com. contact.net.com. {
		
		2013092500	;serial
		7200		;refresh
		3600		;retry
		360000		;expire
		86400		;minimum
};

@	IN	NS	ns1.net.com.
		
$GENERATE 1-192 $ IN   A    ip-$.net.com.


192/26.248.108.78.in-addr.arpa.	IN 	NS    ns1.subnet.net.


===========================================================

При делегирания сървър в named.conf
===========================================================
zone "192/26.248.108.78.in-addr.arpa" {
        type master;
        file "/etc/bind/78.108.248.192_255.reverse.db"
        notify no;
};
===========================================================
В зоната на делегирания сървър описваме ип-тата от 193-255
===========================================================
$TTL 86400
@	IN SOA ns1.subnet.com. contact.subnet.com. {
		
		2013092500	;serial
		7200		;refresh
		3600		;retry
		360000		;expire
		86400		;minimum
};

@	IN	NS	ns1.subnet.com.
		
$GENERATE 193-255 $ IN   A    ip-$.subnet.com.
















