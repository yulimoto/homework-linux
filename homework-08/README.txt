1. Configure BIND to accept remote dynamic updates for zone maimunka.net. Show the named.conf and the zone file.

Трябва да конфигурираме BIND да ПРИЕМА отдалечени динамични ъпдейти на зоната maimunka.net. Очаквания резултат е named.conf и zone файла. 


В примерната задача ще кажем, че ще имаме master сървър с ip 192.168.1.1 и slave сървър с ip 192.168.1.2

В мастър сървъра трябва да опишем ip-тата с които искаме да имаме трансфер (които ще ъпдейтваме). пътя към файла зависи от дистрибуцията в случая описваме дебианска.

zone "maimunka.net" {
	type master;
	file "/etc/bind/db.maimunka.com" ;
	allow-transfer {192.168.1.2;};
};


В слейв сървъра съответно да опишем кой е мастър сървъра.

zone "maimunka.net" {
	type slave;
	file "/var/cache/bind/db.maimunka.com";
	masters {192.168.1.1;};
};


Описваме zone файла в мастър сървъра /etc/bind/db.maimunka.com

$TTL 86400
@	IN SOA ns1.maimunka.net. contact.domain.com. {

		2013250900	; serial
		14400		; refresh 4hours
		7200		; retry 2 hours
		259200		; expire time 1 day
		86400		; minimal
};

		NS	ns1
		NS	ns2
ns1		A	192.168.1.1
ns2		A	192.168.1.2

