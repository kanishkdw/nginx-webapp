# Nginx Web-App

Nginx is a high-performance, open-source web server, reverse proxy server, and load balancer.

The basic architecture of a Docker image is a Dockerfile.
Dockerfile is a text file comprising instructions and arguments .
By these Dockerfile , docker can easily build the dockerimage.

in the sourcecode everything written on the left is the instruction and on the right are the arguments for those instruction.

- Lets build a dockerimage using a Dockerfile----

Step 1. Create files and folders----

        cd nginx-webapp
        mkdir files

Step2. Create a sample Html file and config file---

When you build a docker image for real-time projects, it contains code or application config files.
Lets create a sample index.html file

        cd firstwebsite
        vi index.html
this lets u enter in the firstwebsite folder and vi command lets to write the code;

        <html>
        <head>
            <title>Dockerfile</title>
        </head>
        <body>
            <div class="container">
            <h1>My App</h1>
            <h2>This is my first app</h2>
            <p>Hello everyone, This is running via Docker container</p>
            </div>
        </body>
        </html>

this is the source code and add this in the file and save it by pressing the esq+shift+; and write wq then pressing enter.

Now create a another file naming it as default ;

        vi default

after entering the folder write a source code in it;

        server {
            listen 80 default_server;
            listen [::]:80 default_server;
            
            root /usr/share/nginx/html;
            index index.html index.htm;

            server_name _;
            location / {
                try_files $uri $uri/ =404;
            }
        }

Step 3. Choose a Base image---

FROM command is used to instruct Docker to create an image based on an image that is available on the docker hub ....

Create a Dockerfile in the nginx-webapp folder.

        vi Dockerfile

now just add this samplefile as a content for our use case. 

        FROM ubuntu:18.04  
        LABEL maintainer="contact@devopscube.com" 
        RUN  apt-get -y update && apt-get -y install nginx
        COPY files/default /etc/nginx/sites-available/default
        COPY files/index.html /usr/share/nginx/html/index.html
        EXPOSE 80
        CMD ["/usr/sbin/nginx", "-g", "daemon off;"]

Step 4. Build the Dockerimage


        docker build -t nginx:1.0 .

Code Explaination-
1. -t is for tagging the image.
2. nginx is the name of the image.
3. 1.0 is the tag name. If you don’t add any tag, it defaults to the tag named latest.
4. . (dot) at the end means, we are referring to the Dockerfile location as the docker build context.

for check list the images by using
        docker images


Step5. test the Docker image----

        docker run -d -p 9090:80 --name webserver nginx:1.0

check the container by using the below command;

    docker ps

Now in the browser,go to http://<host-ip>:9090 or type localhost:9090, you can see the index page which displays the content in the custom HTML page we added to the docker image.

![Screenshot 2024-07-18 161430](https://github.com/user-attachments/assets/c15ef801-d813-452b-97c3-6e4b69d78cfc)


#Push Docker Images to Docker hub----

Activating the Docker hub

                docker login
![image](https://github.com/user-attachments/assets/ff473028-6ead-4131-92d8-bea73c5fb534)



After succesfull login , just tag the image with the docker username as shown ----

                docker tag nginx:1.0 <username>/<image-name>:tag

Example;

                docker tag nginx:1.0 <username>/nginx:1.0

For the confirmation, just run the docker image command and check there;

Now push the image to docker hub ;

                docker push username/nginx:1.0

Just check the image at the repository tab at the docker hub;
