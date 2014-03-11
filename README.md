Partial-Reconfiguration-of-Software-Defined-Radio
=================================================

My Under- Graduate Project (Verilog implementation)
module decoder_3_8(b,a,i,q); // Decoder Block
    input [2:0]i,q;
    output [2:0]b,a;
    reg [2:0]b,a;
  always @*
  begin
   case (i)
        3'b000 : b=3'b000;
        3'b001 : b=3'b100;
        3'b010 : b=3'b110;
        3'b011 : b=3'b010;
        3'b100 : b=3'b011;
        3'b101 : b=3'b111;
        3'b110 : b=3'b101;
        3'b111 : b=3'b001;
    endcase
    end
    always @*
  begin
   case (q)
        3'b000 : a=3'b000;
        3'b001 : a=3'b100;
        3'b010 : a=3'b110;
        3'b011 : a=3'b010;
        3'b100 : a=3'b011;
        3'b101 : a=3'b111;
        3'b110 : a=3'b101;
        3'b111 : a=3'b001;
    endcase
    end
   endmodule

module test_decoder_3_8(); // The test bench for the decoder
reg [2:0]i,q;
wire [2:0]b,a;
decoder_3_8  dec0(b,a,i,q);
initial
begin
     i=3'b000; 
     q=3'b001 ;
     #100 i=3'b010;
        q=3'b101;
end
initial
$monitor ($time," dec_input = %b %b  dec_output=%b%b",i,q,b,a);
endmodule

module qam16( clk,enable,data, md_type,Iout,Qout,we,ready2);// Quad Amplitude Modulation

input clk,enable;
input data;          
output [1:0] md_type;
output reg we;
output ready2;
reg [1:0]count=0;
reg [3:0] iq;
output reg [15:0] Iout=0, Qout=0;

parameter [1:0]qam16=2'b01;
assign md_type=qam16;

always @(posedge clk)
if(!enable)
count<=0;
else
begin 
count<=count+1;
if(count==3)
count<=0;
end


always @(posedge clk)
if(enable)
iq<={data,iq[3:1]};

	
always @(posedge clk)
if(count==3)
begin
case ({iq[0],iq[1]})
2'b00: Iout= -31086;
2'b01: Iout= -10362;
2'b11: Iout= 10362;
2'b10: Iout= 31086;
endcase

case ({iq[2],iq[3]})
2'b00: Qout= -31086;
2'b01: Qout= -10362;
2'b11: Qout= 10362;
2'b10: Qout= 31086;
endcase

we<=1;
end
else 
begin
we<=0;
Iout=0 ; Qout=0;
end

assign ready2=1;
endmodule


module qam64( clk,enable,data, md_type,Iout,Qout,we,ready2);// Quad Amplitude Modulation 64 bit

input clk,enable;
input data;
output [1:0] md_type;

reg [2:0]count=0;
reg [5:0] iq;
output reg [15:0] Iout=0, Qout=0;
output reg we;
output ready2;
parameter [1:0]qam6=2'b10;
assign md_type=qam6;

always @(posedge clk)
if(!enable)
count<=5;
else
begin 
count<=count+1;
if(count==5)
count<=0;
end


always @(posedge clk)
if(enable)
iq<={data,iq[5:1]};

	
always @(posedge clk)
if(count==5)
begin
case(iq[2:0])
3'b0: Iout=-35393;
3'd1: Iout=-25281;
3'd3: Iout=-15168;
3'd2: Iout =-5056;
3'd6: Iout=5056;
3'd7: Iout=15168;
3'd5: Iout=25281;
3'd4: Iout=35393;
endcase

case (iq[5:3])
3'b0: Qout=-35393;
3'd1: Qout=-25281;
3'd3: Qout=-15168;
3'd2: Qout =-5056;
3'd6: Qout=5056;
3'd7: Qout=15168;
3'd5: Qout=25281;
3'd4: Qout=35393;
endcase
we<=1;
end
else
we<=0;
 
assign ready2=1; 
endmodule

module qpsk(clk,enable,data,md_type,Iout,Qout,we,ready2);// Quad Phase Shift Keying

input clk,enable;

input Iout, Qout;

output [1:0] md_type;

reg [1:0]count=0;

reg [3:0] inp1, inp2;

output reg [1:0] d1out=0, d2out=0;

output reg we;

output ready2;

parameter [1:0]qam6=2'b10;

assign md_type=qam6;


always @(posedge clk)

  if(!enable)

 count<=1;

  else

begin 

  count<=count+1;

  if(count==1)

  count<=0;

end
always @(posedge clk)

begin


if(enable)
begin

inp1<={Iout,inp1[1:0]};


inp2<={Qout,inp2[1:0]};
end
end
always @(posedge clk)

if(count==1)

begin
case(inp1[1:0])
2'd-1: d1out=1'b0;

2'd1: d1out=1'b1;
endcase
case(inp2[1:0])
2'd-1: d2out= 1'b0;
2'd1: d2out=1'b1;
endcase
we<=1;
end
else
we<=0;
 assign ready2=1; 
endmodule

module viterbi_encode(X2N,X1N,Y2N,Y1N,Y0N,clk,res);
input X2N,X1N,clk,res; output Y2N,Y1N,Y0N;
wire X1N_1,X1N_2,Y2N,Y1N,Y0N;
dff dff_1(X1N,X1N_1,clk,res); dff dff_2(X1N_1,X1N_2,clk,res);
assign Y2N=X2N; assign Y1N=X1N ^ X1N_2; assign Y0N=X1N_1; 
endmodule 

module pcr_test(); // PCR test
    reg clk;
    reg [15:0] b ;
    wire [15:0] a;
    pcr d1(clk,b,a);		
    initial
    begin
   
    clk=0; #5 clk=1; b=4'd01;
    #5 clk=0; #5 clk=1; b=4'd02;
    #5 clk=0; #5 clk=1; b=4'd03;
#5 clk=0; #5 clk=1;    b=4'd04;
#5 clk=0; #5 clk=1;    b=4'd05;
#5 clk=0; #5 clk=1;    b=4'd06;
#5 clk=0; #5 clk=1;    b=4'd07;
#5 clk=0; #5 clk=1;    b=4'd08;
#5 clk=0; #5 clk=1;    b=4'd09;
#5 clk=0; #5 clk=1;    b=4'd10;
#5 clk=0; #5 clk=1;    b=4'd11;
#5 clk=0; #5 clk=1;    b=4'd12;
#5 clk=0; #5 clk=1;    b=4'd13;
#5 clk=0; #5 clk=1;    b=4'd14;
#5 clk=0; #5 clk=1;    b=4'd15;
#5 clk=0; #5 clk=1;    b=4'd16;
#5 clk=0; #5 clk=1;    b=4'd17;
#5 clk=0; #5 clk=1;    b=4'd18;
#5 clk=0; #5 clk=1;    b=4'd19;
#5 clk=0; #5 clk=1;    b=4'd20;
#5 clk=0; #5 clk=1;    b=4'd21;
#5 clk=0; #5 clk=1;    b=4'd22;
#5 clk=0; #5 clk=1;    b=4'd23;
#5 clk=0; #5 clk=1;    b=4'd24;
   #5 clk=0; #5 clk=1; b=4'd02;
   #5 clk=0; #5 clk=1; b=4'd03;
#5 clk=0; #5 clk=1;    b=4'd04;
#5 clk=0; #5 clk=1;    b=4'd05;
#5 clk=0; #5 clk=1;    b=4'd06;
#5 clk=0; #5 clk=1;    b=4'd07;
#5 clk=0; #5 clk=1;    b=4'd08;
#5 clk=0; #5 clk=1;    b=4'd09;
#5 clk=0; #5 clk=1;    b=4'd10;
#5 clk=0; #5 clk=1;    b=4'd11;
#5 clk=0; #5 clk=1;    b=4'd12;
#5 clk=0; #5 clk=1;    b=4'd13;
#5 clk=0; #5 clk=1;    b=4'd14;
#5 clk=0; #5 clk=1;    b=4'd15;
#5 clk=0; #5 clk=1;    b=4'd16;
#5 clk=0; #5 clk=1;    b=4'd17;
#5 clk=0; #5 clk=1;    b=4'd18;
#5 clk=0; #5 clk=1;    b=4'd19;
#5 clk=0; #5 clk=1;    b=4'd20;
#5 clk=0; #5 clk=1;    b=4'd21;
#5 clk=0; #5 clk=1;    b=4'd22;
#5 clk=0; #5 clk=1;    b=4'd23;
#5 clk=0; #5 clk=1;    b=4'd24;
#5 clk=0; #5 clk=1;    b=4'd05;
#5 clk=0; #5 clk=1;    b=4'd06;
#5 clk=0; #5 clk=1;    b=4'd07;
#5 clk=0; #5 clk=1;    b=4'd08;
#5 clk=0; #5 clk=1;    b=4'd09;
#5 clk=0; #5 clk=1;    b=4'd10;
#5 clk=0; #5 clk=1;    b=4'd11;
#5 clk=0; #5 clk=1;    b=4'd12;
#5 clk=0; #5 clk=1;    b=4'd13;
#5 clk=0; #5 clk=1;    b=4'd14;
#5 clk=0; #5 clk=1;    b=4'd15;
#5 clk=0; #5 clk=1;    b=4'd16;
#5 clk=0; #5 clk=1;    b=4'd17;
#5 clk=0; #5 clk=1;    b=4'd18;
#5 clk=0; #5 clk=1;    b=4'd19;
#5 clk=0; #5 clk=1;    b=4'd20;
#5 clk=0; #5 clk=1;    b=4'd21;
end
initial
$monitor ($time," dec_input = %b  dec_output = %b",b,a);

endmodule 

