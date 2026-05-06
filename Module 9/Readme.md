### Deploying docker containers

- **Production vs Development environment**
  - <img width="720" height="403" alt="Screenshot 2026-05-06 211953" src="https://github.com/user-attachments/assets/9c2df450-aeea-4a51-b7d6-f80187c34c17" />
- **Deploying application as container**
  - <img width="720" height="402" alt="Screenshot 2026-05-06 212023" src="https://github.com/user-attachments/assets/25eca872-3cf2-4f0c-973f-b21777f84de1" />
  - We can use providers like AWS, Azure, or GCP or any other provider that supports container/ docker hosting
- **Example use of AWS**
  - We can deploy an EC2 instance
  - Install docker in it
  - Build our image locally and push it to our repository like `DockerHub`
  - Use the image in running container on EC2 using `SSH`
