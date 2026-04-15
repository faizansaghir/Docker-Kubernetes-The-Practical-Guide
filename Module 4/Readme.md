### Containers and Network Requests

- **Type of communication**
  - <img width="720" height="405" alt="Screenshot 2026-04-15 191353" src="https://github.com/user-attachments/assets/060c84a1-109a-4ec5-8478-9e1d5e7fcd31" />
- **Netowrk request from container to WWW***
  - Out of the box, containers are able to communicate with WWW and needs to special setup.
  - We can simply use `curl` or `axios.get` or other such shell or language specific commands to communicate to the WWW
- **Network request from container to Host machine**
  - If we use `localhost` as our address/ domain eg: `mongodb://localhost:27017/swfavorites` for connecting to some service running on host machine, it will not be able to connect to it
  - To make sure container code is able to connect to service running on host machine, use `host.docker.internal` as address/ domain eg: `mongodb://host.docker.internal:27017/swfavorites`.
  - Docker resolves `host.docker.internal` to translate to IP of the host VM
