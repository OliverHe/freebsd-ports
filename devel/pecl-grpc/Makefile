# Created by: vanilla@

PORTNAME=	grpc
PORTVERSION=	1.41.0
CATEGORIES=	devel net

MAINTAINER=	vanilla@FreeBSD.org
COMMENT=	High performance, general RPC framework

LICENSE=	APACHE20
LICENSE_FILE=	${WRKSRC}/LICENSE

BROKEN_mips=		fails to compile: fatal error: sys/auxv.h file not found
BROKEN_mips64=		fails to compile: fatal error: sys/auxv.h file not found

USES=		compiler:c++11-lang php:pecl

.include <bsd.port.pre.mk>

.if ${FLAVOR} == php72 && ${ARCH} == i386
BROKEN=		error: fastcall calling convention ignored on variadic function [-Werror,-Wignored-attributes]
.endif

.include <bsd.port.post.mk>
