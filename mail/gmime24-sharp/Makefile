# Created by: Tom McLaughlin <tmclaugh@sdf.lonestar.org>

PORTREVISION=	5
CATEGORIES=	mail
PKGNAMESUFFIX=	24-sharp

MAINTAINER=	mono@FreeBSD.org
COMMENT=	Mono bindings for gmime

LIB_DEPENDS=	libgmime-2.4.so:mail/gmime24

DEPRECATED=	Use mail/gmime26-sharp
EXPIRATION_DATE=2021-11-29

MASTERDIR=	${.CURDIR}/../gmime24
BUILD_WRKSRC=	${WRKSRC}/mono
INSTALL_WRKSRC=	${BUILD_WRKSRC}
DESCR=		${.CURDIR}/pkg-descr
PLIST=		${.CURDIR}/pkg-plist

USES=		gnome mono
USE_GNOME=	glib20 gtksharp20
CONFIGURE_ARGS=	--enable-mono=yes

# Else we conflict with mail/gmime24
OPTIONS_EXCLUDE=	DOCS

# Done in pre-configure because master port has a post-patch target already.
pre-configure:
	@${REINPLACE_CMD} -e 's|.@GMIME_VERSION@||g' \
		${WRKSRC}/mono/gmime-sharp.dll.config.in

.include "${MASTERDIR}/Makefile"
