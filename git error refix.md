git error :fatal: unable to access 'https://github.com/linlin-2020/note.git/': OpenSSL SSL_read: Connection was reset, errno 10054
原因：上传的文件太大，缓存不够，默认只有1M
修复：修改缓存大小命令 ---- 》 git config http.postBuffer 524288000
