###  A  

#### 1.duration of A is 0.4s 

#### 2 elements in the A  is 2001

#### 3.change the number of the Fs, I have heard the voice become more sharp

#### 4.code song.m 
<pre class="matlab-code">
clear all 
Fs=5000; %采样频率
Ts=1/Fs; 
t=[0:Ts:0.3]; %音长，播放时间
F_A=440; 
F_B=493.88; 
F_C=554.37;
F_D=587.33;
F_E=659.26;
F_F=739.99;
A=sin(2*pi*F_A*t); 
B = sin(2*pi*F_B*t);
C = sin(2*pi*F_C*t);
D = sin(2*pi*F_D*t);
E = sin(2*pi*F_E*t);
F = sin(2*pi*F_F*t);
s = [A,A,E,E,F,F,E,E,D,D,C,B,B,A,A];
sound(s);
</pre>

### B 

#### 1. help tone is to help us to know the right input format of tone 
#### 2. to ues the funciton of tone to output the sound 

### C 

####  code dtmfdial.m
<pre class='matlab-code'>
function xx=dtmfdial(keyName)
dtmf.keys = ['1','2','3';
             '4','5','6';
            '7','8','9';
            '*','0','#'];

ff_cols = [1209,1336,1477];                      
ff_rows = [697;770;852;941];                    
dtmf.colTones = ones(4,1)*ff_cols; 
dtmf.rowTones = ff_rows*ones(1,3);
fs = 8000;                                       
[ii,jj] = find(keyName==dtmf.keys);
frez_1 = dtmf.colTones(ii,jj);
frez_2 = dtmf.rowTones(ii,jj);
Ts = 1/fs;
t = [0:Ts:0.2];
xx=cos(2*pi*frez_1*t)+cos(2*pi*frez_2*t);
</pre>

#### to use the function
<pre class='matlab-code'>
clear;clc
keyName = ['#'];
xx = dtmfdial(keyName);
soundsc(xx)
</pre>

### D
#### 1. 
<pre class='matlab-code'>
w = 0.2*pi;
L1 = 50;
L2 = 500;
n1 = 0:1:L1-1;
n2 = 0:1:L2-1;
h1 = 1/sqrt(L1)*cos(w*n1);
h2 = 1/sqrt(L2)*cos(w*n2);
[a1,b1] = freqz(h1);
[a2,b2] = freqz(h2);
figure(1);
plot(b1,abs(a1));
figure(2);
plot(b2,abs(a2));
</pre>
##### The L = 50 :
![u9hfu8.png](https://s2.ax1x.com/2019/09/22/u9hfu8.png)

##### The L = 500:
![u9hqg0.png](https://s2.ax1x.com/2019/09/22/u9hqg0.png)

##### When the L become ∞ , I think the shape will bacome a line  in a in an interval,we can get Peak Value more easier


#### 2.
#### We can use matlab code to get results 
#####  WHEN L = 50 
<pre class='matlab-code'>
w = 0.2*pi;
L1 = 50;
L2 = 500;
n1 = 0:1:L1-1;
n2 = 0:1:L2-1;
h1 = 1/sqrt(L1)*cos(w*n1);
h2 = 1/sqrt(L2)*cos(w*n2);
[a1,b1] = freqz(h1);
[a2,b2] = freqz(h2);
Eh1= sum((abs(h1)).^2);
</pre>
##### Eh1 = 0.5000

##### WHEN L = 500 
<pre class='matlab-code'>
w = 0.2*pi;
L1 = 50;
L2 = 500;
n1 = 0:1:L1-1;
n2 = 0:1:L2-1;
h1 = 1/sqrt(L1)*cos(w*n1);
h2 = 1/sqrt(L2)*cos(w*n2);
[a1,b1] = freqz(h1);
[a2,b2] = freqz(h2);
Eh2= sum((abs(h1)).^2);
</pre>
##### Eh2 = 0.5000

#### 3. 

#### L = 500 can get  a better DTMF decoding performance
#### The reason is that the bigger L , we can get Peak Value more easier

### F 
#### The code of  dtmfdetect.m
<pre class='matlab-code'>
function detected = dtmfdetect(keyName,filter_length,noise_power);
dtmf.keys = ['1','2','3';
             '4','5','6';
            '7','8','9';
            '*','0','#'];

ff_cols = [1209,1336,1477];                     
ff_rows = [697;770;852;941];                    
dtmf.colTones = ones(4,1)*ff_cols; 
dtmf.rowTones = ff_rows*ones(1,3);
fs = 8000;                                      
[ii,jj] = find(keyName==dtmf.keys);
frez_1 = dtmf.colTones(ii,jj);
frez_2 = dtmf.rowTones(ii,jj);
Ts = 1/fs;
t = [0:Ts:0.2];
input_s=cos(2*pi*frez_1*t)+cos(2*pi*frez_2*t);

noise = sqrt(noise_power)*randn(1,length(input_s));

noise_tone = input_s + noise;
L = filter_length;
h1 = fiter_6(L,697);
h2 = fiter_6(L,770);
h3 = fiter_6(L,852);
h4 = fiter_6(L,941);
h5 = fiter_6(L,1209);
h6 = fiter_6(L,1336);
h7 = fiter_6(L,1477);

y1 = conv(noise_tone,h1);
y2 = conv(noise_tone,h2);
y3 = conv(noise_tone,h3);
y4 = conv(noise_tone,h4);
y5 = conv(noise_tone,h5);
y6 = conv(noise_tone,h6);
y7 = conv(noise_tone,h7);

enenry1 = sum(y1.*y1);
enenry2 = sum(y2.*y2);
enenry3 = sum(y3.*y3);
enenry4 = sum(y4.*y4);
enenry5 = sum(y5.*y5);
enenry6 = sum(y6.*y6);
enenry7 = sum(y7.*y7);
energy=[enenry1,enenry2,enenry3,enenry4,enenry5,enenry6,enenry7];
B=sort([enenry1,enenry2,enenry3,enenry4,enenry5,enenry6,enenry7]);
fre_max1 = find(B(6)==energy);
fre_max2 = find(B(7)==energy);
fre_list = [697,770,852,941,1209,1336,1477];
row_frequence1 = fre_list(min(fre_max1,fre_max2));
clos_frequence2 = fre_list(max(fre_max1,fre_max2));

ff_cols = [1209,1336,1477];                      
ff_rows = [697;770;852;941];                    

dtmf.keys = ['1','2','3';
             '4','5','6';
            '7','8','9';
            '*','0','#'];
[location_1,~] = find(row_frequence1==ff_rows);
[~,location_2 ] = find(clos_frequence2==ff_cols);
key_result=dtmf.keys(location_1,location_2);

if key_result == keyName
    print('The detected key is %d',key_result)
else
    print('error')
end
</pre>

#### the function of fiter_6.m
<pre class='matlab'>
function h = fiter_6(L,center_frequency);

n = 0:1:L-1;

h = 1/sqrt(L)*cos(2*pi*center_frequency*n/8000);

end
</pre>

### the result
![u9xHP0.png](https://s2.ax1x.com/2019/09/22/u9xHP0.png)