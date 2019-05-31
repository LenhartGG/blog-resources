1.登录putty
 
- 135.252.218.139

- usernamee：zenwenh

- pwd：nsb@38434745

2.进入docker

- $docker exec -it iPluggableeNode sh;
- # cd /
- # cd iPluggable.2019.04.02
- # git pull

根据PID杀死原来的服务并重启
 
- ps -ef
- kill PID
- npm start