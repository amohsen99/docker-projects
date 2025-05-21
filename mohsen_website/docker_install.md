# docker install my_website

# 1. Install Docker
install docker via script 

# 3. Clone the repository
git clone https://github.com/amohsen99/my_website.git


# 5. Run the Docker container
docker run -it -d --name mohsen_website -p 8080:80 --name web -v ~/Documents/kit/docs/cloud/devops/docker/projects/my_website:/usr/share/nginx/html nginx
