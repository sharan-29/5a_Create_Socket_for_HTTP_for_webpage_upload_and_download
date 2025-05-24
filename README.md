# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
```
import socket

def send_request(host, port, request):
    try:
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
            s.connect((host, port))
            s.sendall(request.encode())
            response = s.recv(4096).decode()
        return response
    except Exception as e:
        return str(e)

def upload_file(host, port, filename):
    try:
        with open(filename, 'rb') as file:
            file_data = file.read()
            content_length = len(file_data)
            request = f"POST /upload HTTP/1.1\r\nHost: {host}\r\nContent-Length: {content_length}\r\n\r\n"
            request += file_data.decode('utf-8', 'ignore')  # Properly encode file data
            response = send_request(host, port, request)
            return response
    except FileNotFoundError:
        return "File not found."
    except Exception as e:
        return str(e)

def download_file(host, port, filename):
    try:
        request = f"GET /{filename} HTTP/1.1\r\nHost: {host}\r\n\r\n"
        response = send_request(host, port, request)
        # Assuming the response contains the file content after the headers
        if "200 OK" in response:  # Check if download was successful
            file_content = response.split('\r\n\r\n', 1)[1]
            with open(filename, 'wb') as file:
                file.write(file_content.encode())
            return "File downloaded successfully."
        else:
            return "Failed to download file."
    except Exception as e:
        return str(e)

if __name__ == "__main__":
    host = 'example.com'
    port = 80

    # Upload file
    upload_response = upload_file(host, port, 'example.txt')
    print("Upload response:", upload_response)

    # Download file
    download_response = download_file(host, port, 'example.txt')
    print(download_response)

```
## OUTPUT
![441355731-24cf0e05-8d06-403d-8c70-49ef9de0defb](https://github.com/user-attachments/assets/da70531d-0bde-4ca8-bab0-9893170ed29d)


## Result
Thus the socket for HTTP for web page upload and download created and Executed
