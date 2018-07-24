```
Traceback (most recent call last):
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\pymongo\pool.py", line 823, in _get_socket_no_auth
    sock_info, from_pool = self.sockets.pop(), True
KeyError: 'pop from an empty set'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\pymongo\pool.py", line 743, in connect
    sock = _configured_socket(self.address, self.opts)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\pymongo\pool.py", line 645, in _configured_socket
    sock = _create_connection(address, options)
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\pymongo\pool.py", line 629, in _create_connection
    raise err
  File "C:\Users\geekview\Anaconda3\envs\GeekParity\lib\site-packages\pymongo\pool.py", line 622, in _create_connection
    sock.connect(sa)
OSError: [WinError 10048] 通常每个套接字地址(协议/网络地址/端口)只允许使用一次。
AutoReconnect: localhost:27017: [WinError 10048] Only one usage of each socket address (protocol/network address/port) is normally permitted
```

