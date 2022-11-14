# NETWORKING

The base of networking is to allow players to play on the same party. We can achieve this with the help of ASIO :
<ul>
    <li>cross-platform C++ library</li>
    <li>Low-level I/O programing</li>
    <li>Asynchronous model using modern C++</li>
</ul>
With this methode we can build a server using IPC (Inter Process Communication) with a UDP/IP network.

The goal of our networking is give players the possibility to host their parties and play together.

## SERVER USAGE

You must disable the firewall on the machine running the server in order to enable the communinications between multiple computers on the same network using IPv4.  

#### On Fedora : 

> sudo systemctl stop firewalld

#### On Ubuntu :

> sudo ufw disable

You can start the server on any unused port using the command :

> sudo ./bin/r-type_server <port<port>>

-----------------------------------------------------------------------------------

## CLIENT USAGE

If you want to play with an other computer on the same Network, you want to know the IPv4 of the machine running the server, you can get it by running the following command on the server's machine :

> ip -4 addr

You're looking for an IP with the attributes : <BROADCAST,MULTICAST,UP,LOWER_UP> 

Now in order to start the client you'll have to run the command :

> ./bin/r-type_client

-----------------------------------------------------------------

# Base Structure :
    Struct baseStruct {
        int code_
        void *data_
    }

# TCP

    from Client to Server

        100 Join server
    
        101 Client is Ready
    
        102 Client has finished loading
    
        103 Debug print to console


    from Server to client

        200 Everyone is Ready

        201 Everyone has finished loading

        202 Player x left

        203 Player x Ready

        204 Player x joined

        205 Clients must start loading

        206 Debug print to console
    
# UDP

    from Client to Server

        300 Update client inputs

        301 Debug print to console


   from Server to Client

        400 Sprite List to draw

        401 Game Over

        402 Debug print to console
        
# Associated structs

100, 101, 102, 200, 201 : data_ is NULL

103, 206, 301, 402 : data_ is a char *

202, 203, 204 : data_ is an int

205, 400 : data_ is Entity *

401 : data_ is long *
