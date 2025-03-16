##### docker file
**node.js**
- Change the folder where the package exist 
- Change the main run point :  CMD [ "node", "new_main_file.js" ]
- Remeber to change the exposed Port in dockerfile to match the app
**node.php**
change THe Paths


---
##### Nginx.Config
- Make sure the container is on the same network 
- In nginx.conf that the ports is the internal ports of the apps
- 