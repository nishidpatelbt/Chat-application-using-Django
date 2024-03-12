# Chat Application Using Django

# Chat App  #

Develop a compact, yet fully functional person-to-person message center application using Django. The application features a REST API and utilizes WebSockets to notify clients of new messages, eliminating the need for constant polling.

## Building Design ##
 - Upon user login, the frontend retrieves the user list and establishes a WebSocket connection to the server, specifically to the notifications channel.
 - When a user selects another user to chat with, the frontend fetches the latest 15 messages (as per settings) exchanged between them.
 - When a user sends a message, the frontend initiates a POST request to the REST API. Subsequently, Django saves the message and notifies the users involved via the WebSocket connection, sending the new message ID.
 - Upon receiving a new message notification (including the message ID), the frontend executes a GET query to the API to download the received message.

### Entreaty ###
"With Channels, Django transitions into a multi-process model. This means you no longer operate everything within a single process alongside a WSGI server (although you still have the option to do so if Channels are not utilized). Instead, you run one or more interface servers, along with one or more worker servers, which are connected via the configured channel layer."

In this setup, the In-Memory channel system is initially used for simplicity. However, to enhance performance and support scalability in a distributed environment, the channel layer backend can be switched to Redis. By utilizing Redis as the backend for the channel layer, multiple workers can be spawned, enabling efficient handling of WebSocket connections and asynchronous tasks across distributed servers. This architectural change ensures optimal performance and scalability for the person-to-person message center application.

![image](https://awesomescreenshot.s3.amazonaws.com/image/1070634/46713261-8395f07e4d3db73c26695186e903e274.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAJSCJQ2NM3XLFPVKA%2F20240312%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240312T183116Z&X-Amz-Expires=28800&X-Amz-SignedHeaders=host&X-Amz-Signature=a45912f675c5253d8a5f40945165d0c9dfc9aa9b464b791f1f0bf769f2f0880e)

### Database ###
For this demonstration, I have opted for a straightforward MySQL setup. However, for increased performance demands, deploying a MySQL cluster or shard can be considered.

We are using indexes to improve performance.

## Run ##

0. move to project root folder


1. Create and activate a virtualenv (Python 3)
```bash
pipenv --python 3 shell
```
2. Install requirements
```bash
pipenv install
```
3. Create a MySQL database
```sql
CREATE DATABASE chat CHARACTER SET utf8;
```
4. Start Redis Server
```bash
redis-server
```

5. Init database
```bash
./manage.py migrate
```
6. Run tests
```bash
./manage.py test
```

7. Create admin user
```bash
./manage.py createsuperuser
```

8. Run development server
```bash
./manage.py runserver
```

To override default settings, create a local_settings.py file in the chat folder.

Message prefetch config (load last n messages):
```python
MESSAGES_TO_LOAD = 15
```
