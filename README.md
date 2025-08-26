# FIFO-Project
Designed a parameterized Single Clock FIFO in Verilog using Vivado, featuring synchronous read/write, circular buffering, and accurate full/empty flag handling. Developed a comprehensive testbench to validate functionality and edge cases. Gained hands-on experience in FIFO design, simulation, and debugging using Vivado’s waveform viewer.

THIS IS THE TEST BENCH OF THE VERILOG CODE OF SINGLE FIRST IN FIRST OUT CODE!!!!

module tbfifo( );


  reg clk;
  reg rst;
  reg wr_en;
  reg rd_en;
  reg [7:0] buf_in;


  wire [7:0] buf_out;
  wire buf_empty;
  wire buf_full;
  wire [7:0] fifo_counter;

 
  clkfifomain uut (
    .rst(rst),
    .clk(clk),
    .wr_en(wr_en),
    .rd_en(rd_en),
    .buf_in(buf_in),
    .buf_out(buf_out),
    .buf_empty(buf_empty),
    .buf_full(buf_full),
    .fifo_counter(fifo_counter)
  );
 always #5 clk = ~clk; 
 initial begin
    clk = 0;
    rst = 1;
    wr_en = 0;
    rd_en = 0;
    buf_in = 0;
#10 rst = 0;
repeat (5) begin
     @(posedge clk);
      wr_en = 1;
      buf_in = buf_in + 8'b11;
    end
    wr_en = 0;

 #10 repeat (3) begin
      @(posedge clk);
      rd_en = 1;
    end
    rd_en = 0;

#10 repeat (2) begin
      @(posedge clk);
      wr_en = 1;
      rd_en = 1;
     buf_in = buf_in + 8'b10;
    end
    wr_en = 0;
    rd_en = 0;

    #10 $finish;
  end

endmodule
