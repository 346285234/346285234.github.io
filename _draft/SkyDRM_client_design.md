SkyDRM客户端架构

不同平台的client，如iOS，Mac...（UI和针对上层客户端需要的功能，比如记录所有文件操作）

sdk（用户登陆登出，云盘管理，包括针对不同云盘的各类操作，获取）

core（文件处理，主要是加密和解密，restapi request和response解析）



rmdsdk架构

功能：用户管理，云盘管理（通过当前用户管理outbox，myvault,、project、workspace和第三方云盘），

Session(管理用户)

User（管理manager，获取preference）

Manager（具体业务，分成显示manager（各类云盘）和隐示manager（token，coredata，downloader，uploader））



UI workflow

登陆-》主界面-〉云盘列表-》文件操作返回界面

