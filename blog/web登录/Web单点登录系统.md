# Web单点登录系统
翻译：lenhart
## 摘要:
目前，许多web应用程序要求用户注册一个新帐户。随着web应用程序的普及，期望用户记住每个应用程序的不同用户名和密码已经变得不切实际。Web单点登录(Web SSO)协议允许用户使用单个用户名和密码访问不同的应用程序。本文研究了三种Web SSO协议:SAML Web浏览器SSO配置文件、WS-Federation被动请求者配置文件和OpenID。

## 关键词:
单点登录、Web单点登录、Web SSO、基于浏览器的SSO、联合身份管理、身份联合、SAML、WS-Federation、OpenID

## 1. 介绍
今天，web应用程序的普及迫使用户记住每个应用程序的多个身份验证凭据(用户名和密码)。面对记住多个凭证这一不切实际的任务，用户重用相同的密码，选择弱密码，或者保留所有用户名和密码的列表。管理多个身份验证凭据对用户来说很麻烦，并且削弱了身份验证系统的安全性。Web单点登录(Web SSO)系统允许对不同的Web应用程序使用单个用户名和密码。对于用户来说，Web SSO系统有助于创建所谓的联合标识。

联合身份管理对用户和应用程序提供者都有好处。用户只记住一个用户名和密码，所以他们不必遭受密码失忆。应用程序提供者还可以降低用户管理成本。它们既不需要支持冗余注册过程，也不需要处理一次性用户创建许多孤立账户的问题。因此，所有涉众都受益于Web SSO标准。Microsoft的Passport协议[Microsoft01]是创建Web SSO标准的第一次尝试，但是该协议从未被非Microsoft供应商广泛部署。[Kormann00]发现护照存在重大安全漏洞，这可能阻碍了护照的采用。

护照不是唯一的漏洞的Web单点登录协议,缺陷在其他协议被确定[Pfitzmann03, Groß03]。虽然Web SSO在概念上类似于Kerberos之类的单点登录(SSO)协议，但是它们必须在商业浏览器的限制下操作。Web SSO系统是基于代理的真实SSO系统[Pashalidis03]。Web SSO协议也称为基于浏览器的或零足迹协议，因为它们在Web浏览器的约束下运行。此外，在基于浏览器的协议中交换的消息必须封装在所有商业浏览器都支持的超文本传输协议(HTTP)中，因为不应该期望用户安装支持新协议的软件。此外，浏览器cookie被绑定到一个域，因此不能用于多域SSO (MDSSO)。

MDSSO是指在不同组织操作的安全域之间发生SSO的情况。例如，在基于web的MDSSO中，Alice使用浏览器在airlineinc.com预订航班，并透明地登录hotelinc.com预订房间。两个域之间必须存在信任关系，以支持这种联合身份验证，因为域可能具有不同的安全策略。本文不涉及信任建立问题。它也不关注属性和授权数据的联合。它处理基于web的MDSSO案例中用户的联合身份验证。

安全分析的基于web的MDSSO协议要求非常规需求和模型,和到目前为止只有一个协议证明密码安全[Groß05]。在本文中,我们集中在三方Web SSO身份验证协议,因为这些协议,分析了[Groß03,Groß05]。这三方包括服务提供者(SP)、身份提供者(IP)和用户代理(UA)。UA是用户操作的web浏览器，IP对UA进行身份验证，SP是web应用程序。

### 1.1相关工作
与MDSSO基于Web的系统不同，一些Web SSO系统在单个组织中提供SSO。它们被称为Web初始登录(WebISO)系统。Internet2 WebISO工作组[WebISO03]试图标准化这些系统，但该项目目前处于非活跃状态。WebISO系统通常为组织现有的单点登录基础设施提供基于web的组件，目前该基础设施不支持基于web的身份验证。组织可以直接控制其安全策略、身份验证过程和对用户数据库的直接访问。因此，在WebISO系统中，跨领域的问题(如建立信任关系)不是主要关注的问题。许多大学已经开发并部署了定制的WebISO系统[WebISO03]。另一个值得注意的互联网项目是shibbo列斯[Shibboleth07]。Shibboleth在一个组织(通常是一所大学)内同时处理基于web的MDSSO和SSO，并且is会实现SAML v1.1。
除了联合身份验证之外，MDSSO联合身份管理还涉及基于属性的授权、隐私伪标识符、单点注销和IP发现。通常，大多数Web SSO协议与联邦身份验证一起解决这些问题。SSO协议还包括在组织之间建立信任关系的机制。
### 1.2组织
本文的第2部分概述了安全性断言标记语言(SAML)标准，作为后面部分介绍其他标准的参考框架。第3节特别描述SAML Web浏览器SSO配置文件。Web服务联盟语言(ws - Federation)被动的请求者,一个Web SSO安全Groß05 Web单点登录协议,是在第四节中描述。OpenID是一种开源的Web SSO协议，在第5节中介绍。我们在第6节以一些结束语结束。

回到目录

### 2. SAML
结构化信息标准促进组织(OASIS)的安全服务技术委员会(SSTC)开发了安全协会标记语言(SAML)。OASIS是一个全球性的组织，它产生的web标准比任何其他组织都多[OASIS]。SAML基于可扩展标记语言(XML)，用于在web域之间通信身份验证和授权数据。它可以说是联邦身份管理的事实标准。成功的SAML实现存在于工业、政府和学术界[Wisniewski05]。许多身份管理产品支持SAML。此外，其他标准，如Internet2 Shibboleth项目、Liberty Alliance、OASIS Web服务安全(WS-Security)和可扩展访问控制标记语言(XACML)都基于SAML [Wisniewski05]。
SAML框架的模块化设计允许将其组件组合起来以支持各种部署场景。SAML由核心、绑定和配置文件组件组成。图1显示了SAML组件之间的关系。概要组件描述使用SAML的上下文，绑定指定用来封装SAML消息的协议。绑定和概要文件基于SAML核心，SAML核心定义消息格式和通用请求/响应协议。

###2.1核心
其核心包括定义消息语法和语义的安全断言，以及用于传输断言的一般请求/响应协议[Cantor05]。断言是带有关于用户的SAML声明的XML包。例如，身份验证断言可能包含说明如何以及何时对用户进行身份验证的语句。请求/响应协议也在XML中指定。在协议中，请求是对断言的查询，响应返回断言或错误。例如，请求可能是对主体进行身份验证，而响应将是一个身份验证断言，说明身份验证是否成功。SAML核心、断言和协议的封装在另一个常见的底层协议中称为绑定。
### 2.2绑定
SAML绑定指定SAML协议消息如何映射到其他常见协议，如简单对象访问协议(SOAP)或HTTP [Cantor05]。绑定使用标准通信和消息传递协议，以允许自治的符合saml的系统安全地传输消息。SAML或底层协议都支持相互身份验证、消息完整性和机密性。例如，在SOAP绑定中，可以保护SOAP或SAML。XML签名和加密用于应用层安全性，TLS/SSL用于传输层安全性。核心和绑定定义了一个称为概要文件的SAML用例。
### 2.3概要文件
SAML配置文件指定如何在特定的应用程序中使用SAML核心和绑定[Hodges06]。例如，Web浏览器SSO配置文件使用HTTP和SOAP的身份验证请求协议和绑定。存在许多类型的SAML概要文件，但是特定于SSO的概要文件包括Web浏览器SSO概要文件、增强客户端或代理(ECP)概要文件、身份提供者发现概要文件、单注销概要文件和名称标识符管理概要文件[Hughes05]。虽然SAML定义了许多概要文件，但是Web SSO是开发标准[Lockhart06a]的主要原因。

回到目录

##3.SAML Web浏览器SSO配置文件
在SAML v1.1中，Web SSO在两个独立的概要文件中实现:浏览器工件和Brower POST概要文件。在SAML v2.0中，创建了单个Web浏览器SSO配置文件。新概要文件将两个旧概要文件合并为绑定。HTTP重定向、HTTP POST和HTTP工件与身份验证请求协议一起实现Web浏览器SSO配置文件[休斯05]。每个绑定定义封装身份验证断言的不同方法。特别是，可扩展超文本标记语言(XHTML)在HTTP POST和HTTP工件绑定中分别根据值和引用形成传输请求/响应消息。
###3.1框架
尽管绑定决定了实际的消息，但是交换遵循一个通用模型，而不考虑绑定的选择[Lockhart06a]。图2描述了这种通用交换模式。
(1) UA试图访问SP上的资源。假设在SP上未对UA进行身份验证，则SP确定UA的IP。(身份发现配置文件可用于确定UA的IP。)
(2) UA代表SP向IP发送身份验证请求消息。
(3) IP通过某种未声明的方法对UA进行身份验证。
IP通过UA中继消息来响应SP。
(5) SP根据IP响应授予或拒绝访问资源。
要将消息通过UA传递到SP/IP，可以使用HTTP POST、HTTP工件或HTTP重定向绑定。但是，由于响应的长度[Lockhart06a]，(5)中不能使用HTTP重定向绑定。对于ip发起的身份验证，交换从(4)开始。

作为一个真实的例子，在图3中，我们描述了使用HTTP工件绑定部署Web SSO概要文件。工件是对消息的引用。可以解析工件以获取消息的内容。
(1) UA试图访问SP上的资源。
(2)为了在IP上请求SSO服务，SP使用UA作为中间人向ID发出请求工件(对请求消息的引用)。
(3) IP请求SP解析请求工件。
SP用包含原始请求消息[Cantor05a]的消息进行响应。
(5) ID对UA进行身份验证
(6) ID再次使用UA作为中间人向SP发送响应工件。
(7) SP请求ID解析响应工件。
(8) ID返回原始响应的内容。
(9) SP根据IP响应授予或拒绝对资源的访问。
为了安全起见，规范建议在发送原始消息的内容之前，使用安全通道(SSL/TLS)将工件传输到/从UA，而SP和IP使用源身份验证。

### 3.2安全问题
安全分析浏览器/工件在SAML v1.1被[Groß03]了。Groß识别协议包括中间人的几个漏洞,消息泄漏和重放攻击。他提供了对策,建议使用“安全通道,如SSL 3.0或TLS 1.0与单边认证”(Groß03)。Groß还指出,安全通道防止中间人和重播攻击,但没有信息泄漏。尽管他将该协议描述为“联邦身份管理中最精心设计的基于浏览器的协议”，但他得出结论，该协议的安全漏洞使其不适合作为行业标准使用。
SSTC回应[Groß03]的[SSTC05]。他们把一些建议由[Groß03]到SAML V2.0,并添加对策(Groß03)确定的威胁。总的来说，SSTC认为SAML上面操作的底层协议中的漏洞本身并不是SAML规范中的弱点。他们还指出,SAML规范推荐使用/ TLS安全通道,计数器两三个袭击中确定[Groß03]。

回到目录

##4. WS-Federation被动请求者概要文件
与SAML类似，WS-*规范是一个模块化体系结构，旨在提供一个灵活的、可扩展的框架来解决一般的web安全问题[Lockhart06]。Web服务联合语言(WS-Federation Language, WS-Federation)是WS-*规范的一部分，该规范解决了基于浏览器的应用程序的Web SSO问题，特别是针对Web服务的Web SSO问题[IBM07]。IBM、微软和其他公司开发了这一标准。被动请求者概要文件的目标是基于浏览器的SSO和其他支持web的设备，而主动请求者概要文件的目标是基于soap的web应用程序。与SAML一样，WS-Federation可以与不同的传输和应用层协议相结合，以实现各种Web SSO场景。
与SAML Web浏览器SSO配置文件一样，WS-Federation也是基于不同的子组件。WS-Federation使用WS-Security框架、WS-Trust和WS-SecurityPolicy [Goodner07]。由OASIS标准化的WS-Security提供了使用安全令牌保护SOAP消息的机制。WS-SecurityPolicy允许服务描述它们的安全需求，WS-Trust定义用于请求/发出安全令牌的安全令牌服务协议。WS- federation扩展了WS- Trust的安全令牌服务(STS)模型。
### 4.1框架
WS-Federation基于WS-Trust STS模型[Nadalin07]提供Web SSO服务。在STS模型中，各方需要一个安全令牌进行通信。STS分配这些安全令牌[Lockhart06]。在尝试访问资源[Goodner07]之前，UA首先查询SP以确定其安全需求。UA使用WS-SecurityPolicy表达式检查它是否具有有效的令牌。如果UA没有有效的令牌，它将从STS请求一个令牌。由于STS是web服务，它也有自己的安全需求。因此，在为SP请求令牌之前，UA必须向STS提供一个安全令牌。
在WS-federation被动请求者概要文件中，STS扮演IP身份验证UA [Bajaj03]的角色。HTTP POST协议用于传输安全令牌。消息交换模式如图4所示。

- (1) UA试图访问SP上的资源。
- (2)假设UA没有有效的令牌，则将UA重定向到IP。
- (3) IP通过分配一个有效的令牌来验证UA。
- (4) UA向SP提供访问令牌，以获得对资源的访问。


### 4.2安全问题
[Groß05]分析了ws - federation被动的请求者的个人资料和证明这是密码安全。他们的工作是很重要的,因为这是第一次正式的证明任何基于浏览器的联盟协议[Groß05]。他们正式地展示了概要文件提供了身份验证并成功地建立了一个安全通道。在他们的证明中，他们为用户和浏览器创建了新的模型，并将安全SSL视为“blackbox子模块”。
回到目录
## 5. OpenID
与SAML和WS-Federation不同，OpenID是一个开源项目，具有社区驱动的标准化过程。Brad Fitzpatrick最初为他的博客应用程序发明了这个协议。OpenID基金会的使命是“启用和保护社区创建的一切”，现在它支持社区。[OpenID]主要的行业供应商也支持OpenID。例如，AOL是IP或OpenID，而微软的身份管理软件支持OpenID。
SAML和WS-Federation是灵活的面向业务的规范。它们为实现人员提供了许多定制部署的选项。另一方面，OpenID是一个以用户为中心的轻量级协议。OpenID不会从其规范中抽象出任何细节;它提供了Web SSO问题的直接解决方案。
### 5.1框架
OpenID采用分散的体系结构，利用现有的域名服务器(DNS)服务。任何有域名的人都可以选择自己的OpenID IP;没有自己域名的用户可以向他们信任的公司注册。OpenID有效地利用了DNS，与Web SSO一起解决了IP发现问题。在身份验证协议中，用户被分配基于域名的标识符。例如，Alice用她的IP注册了OpenID alice@id-service.com。当她试图访问bob-blog.com上受保护的资源时，她提供了她的OpenID alice@id-service.com。然后bob-blog.com联系id-service.com对Alice进行身份验证。要将多个OpenID标识符关联到一个域，OpenID使用Yardis协议。[Miller06]。
图5显示了没有Yardis协议扩展的OpenID如何实现Web SSO。

- (1) UA试图在SP上访问受保护的资源。
- (2)SP请求用户的OpenID。
- (3) UA向SP提供OpenID。
- (4) SP根据OpenID确定IP的位置，通过将UA重定向到IP向IP发送授权请求。
- (5)如有必要，IP对UA进行认证。
- (6) IP通过UA向SP发送签名身份验证响应。
- (7)警司准许或拒绝进入UA。

在(3)之后，SP可以选择使用Diffie-Hellman建立共享密钥，用于消息的签名和验证。

img

## 6. 总结
SAML和WS-Federation都是相对成熟的标准。主要的工业参与者支持它们，许多身份产品也支持它们[OASIS07, IBM07]。这两种协议都经过独立分析，规范都有很好的文档说明。这些标准中的一个可能会主导Web SSO领域。
另一方面，OpenID得到了微软和AOL的支持，所以它是一个严肃的“标准”。它在成熟和形式主义方面的明显弱点可能是它的长处。作为一种轻量级协议，OpenID具有比其他标准更低的准入门槛。因此，许多在线项目都支持OpenID。此外，开源社区的动态特性，特别是其不那么严格的标准化模型，可能有助于该标准领先于其他行业标准一步。

回到目录


参考文献

[Bajaj03]	Bajaj, S., et al., “WS-Federation: Passive Requestor Profile Version 1.0,” July 2003, http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fedpass/ws-fedpass.pdf

[Cantor05a]	Cantor, S., et al., “Bindings for the OASIS Security Assertion Markup Language (SAML) V2.0. OASIS,” March 2005, http://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf

[Cantor05]	Cantor, S., et al., “Assertions and Protocols for the OASIS Security Assertion Markup Language (SAML) V2.0. OASIS Standard,” March 2005, http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf

[Goodner07]	Goodner, M., “Understanding WS-Federation Version 1.0,” May 2007, http://msdn2.microsoft.com/en-us/library/bb498017.aspx

[Groß03]	Groß, T., "Security analysis of the SAML single sign-on browser/artifact profile”, Computer Security Applications Conference, 2003. Proceedings. 19th Annual , vol., no., pp. 298-307, 8-12 Dec. 2003, http://ieeexplore.ieee.org/document/1254334/

[Groß05]	Groβ, T., Pfitzmann, B., and Sadeghi, A., “Proving a WS-federation 
passive requestor profile with a browser model”, In Proceedings of the 2005 Workshop on Secure Web Services (Fairfax, VA, USA, November 11 - 11, 2005). SWS '05. ACM, New York, NY, 54-64. http://doi.acm.org/10.1145/1103022.1103034

[Hodges06]	Hodges, J., “How to Study and Learn SAML”, September 2006, http://identitymeme.org/doc/draft-hodges-learning-saml-00.html

[Hughes05]	Hughes, J., et al., “Profiles for the OASIS Security Assertion Markup Language (SAML) V2.0,” OASIS, March 2005, http://docs.oasis-open.org/security/saml/v2.0/saml-profiles-2.0-os.pdf

[IBM07]	IBM developerWorks, “Web Services Federation Language website,” May 2007, http://www.ibm.com/developerworks/library/specification/ws-fed/

[Kormann00]	Kormann, D. P. and Rubin, A. D., “Risks of the Passport single signon protocol,” Computer Networks, 33(1–6):51–58, June 2000.

[Lockhart06]	Lockhart, H., et al., “Security Assertion Markup Language (SAML) V2.0 Technical Overview. Working Draft 10,” October 2006, http://www.oasis-open.org/
committees/download.php/20645/sstc-saml-tech-overview-2%200-draft-10.pdf

[Lockhart06]	Lockhart, H., “Web Services Federation Language (WS-Federation) Version 1.1,” December 2006, http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf

[Microsoft01]	Microsoft Corporation: Various .NET Passport documentation (started 1999), in particular Technical Overview, Sept. 2001, and SDK 2 .1 Documentation; http://www.passport.comand http://msdn.microsoft.com/

[Miller06]	Miller, J., “Yadis Specification Version 1.0,” March 2006, http://yadis.org/papers/yadis-v1.0.pdf

[Nadalin07]	Nadalin, A., et al., “WS-Trust 1.3,” OASIS Standard, March 2007, http://docs.oasis-open.org/ws-sx/ws-trust/200512/ws-trust-1.3-os.pdf

[OASIS]	About OASIS, Organization for the Advancement of Structured Information Standards website. http://www.oasis-open.org/

[OASIS07]	OASIS Security Services Security Assertion Markup Language (SAML) TC website, November 2007, http://www.oasis-open.org/committees/security/
[OpenID]	OpenID webstie. http://openid.net/

[Pashalidis03]	Pashalidis, A., Mitchell, C., “A taxonomy of single sign-on systems,” Proceedings, volume 2727 of Lecture Notes in Computer Science, Springer-Verlag, Berlin, July 2003, 249-264.

[Pﬁtzmann03]	B. Pfitzmann03 and M. Waidner, “Analysis of Liberty single-signon with enabled clients. IEEE Internet Computing,” 7(6):38–44, 2003.

[Recordon06]	Recordon, D. and Fitzpatrick, B., “OpenID Authentication 1.1,” May 2006, http://openid.net/specs/openid-authentication-1_1.html

[Shibboleth07]	Shibboleth Internet2 Project, October 2007, http://shibboleth.internet2.edu/

[SSTC05]	SSTC Response to “Security Analysis of the SAML Single Sign-on Browser/Artifact Profile”, January 2005 http://www.oasis-open.org/committees/download.php/11191/sstc-gross-sec-analysis-response-01.pdf

[Wisniewski05]	Wisniewski T., et al., “SAML V2.0 Executive Overview,” March 2005, http://xml.coverpages.org/SAML-ExecOverviewV206-11785-20050310.pdf

[WebISO03]	Web Initial Sign-on (WebISO) website, October 2003, http://middleware.internet2.edu/webiso/

Back to Table of Contents

## List of Acronyms
DNS	Domain Names Server
HTTP	Hypertext Transfer Protocol
ID	Identity provider
MDSSO	Multi-domain Single Sign-On
OASIS	Organization for the Advancement of Structured Information Standards
SAML	Security Assertion Markup Language
SOAP	Simple Object Access Protocol
SP	Service provider
SSL	Secure Sockets Layer
SSO	Single Sign-On
SSTC	Security Services Technical Committee of the Organization
STS	Security Token Service
TLS	Transport Layer Security
UA	User agent
URI	Unique resource identifier
WebISO	Web Initial Single Sign-On
Web SSO	Web Single Sign-On
WS-Federation	Web Services Federation Language
WS-Security	Web Services Security
XHTML	Extensible Hypertext Mark-up Language
XML	Extensible Markup Language
Back to Table of Contents