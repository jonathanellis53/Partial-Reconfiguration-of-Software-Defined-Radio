Partial-Reconfiguration-of-Software-Defined-Radio
=================================================

My Under- Graduate Project (Verilog implementation)
module decoder_3_8(b,a,i,q);
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

