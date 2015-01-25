lib_LTLIBRARIES=libpolarssl.la
libpolarssl_la_LDFLAGS = -version-info $(POLARSSL_LTVERSION) -no-undefined

AM_CFLAGS = \
	$(OPTIONAL_PKCS11_HELPER_CFLAGS)
	
libpolarssl_la_SOURCES= \
	aes.c \
	aesni.c \
	arc4.c \
	asn1parse.c \
	asn1write.c \
	base64.c \
	bignum.c \
	blowfish.c \
	camellia.c \
	ccm.c \
	certs.c \
	cipher.c \
	cipher_wrap.c \
	CMakeLists.txt \
	ctr_drbg.c \
	debug.c \
	des.c \
	dhm.c \
	ecdh.c \
	ecdsa.c \
	ecp.c \
	ecp_curves.c \
	entropy.c \
	entropy_poll.c \
	error.c \
	gcm.c \
	havege.c \
	hmac_drbg.c \
	Makefile \
	md.c \
	md_wrap.c \
	md2.c \
	md4.c \
	md5.c \
	memory_buffer_alloc.c \
	net.c \
	oid.c \
	padlock.c \
	pbkdf2.c \
	pem.c \
	pk.c \
	pk_wrap.c \
	pkcs11.c \
	pkcs12.c \
	pkcs5.c \
	pkparse.c \
	pkwrite.c \
	platform.c \
	ripemd160.c \
	rsa.c \
	sha1.c \
	sha256.c \
	sha512.c \
	ssl_cache.c \
	ssl_ciphersuites.c \
	ssl_cli.c \
	ssl_srv.c \
	ssl_tls.c \
	threading.c \
	timing.c \
	version.c \
	version_features.c \
	x509.c \
	x509_create.c \
	x509_crl.c \
	x509_crt.c \
	x509_csr.c \
	x509write_crt.c \
	x509write_csr.c \
	xtea.c 

AM_CPPFLAGS=-I$(top_srcdir)/include
libpolarssl_la_LIBADD = $(OPTIONAL_PKCS11_HELPER_LIBS)
if WIN32
libpolarssl_la_LIBADD += -lws2_32 -lwinmm -lgdi32 
endif