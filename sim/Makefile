VCS			= vcs
VCS_HOME                = /home/richy/tools/synopsys/install/vcs
VCS_FLAGS               = -fsdb -full64 -cpp g++-4.8 -cc gcc-4.8 -LDFLAGS -Wl,--no-as-needed
VCS_FLAGS		+= -debug_access+all -kdb -lca +libext+.sv +libext+.v -assert svaext
HBUS_HOME 		= /home/richy/projects/uDMA/uDMA_hyperbus/udma_hyperbus
HPFLASH_HOME		= /home/richy/projects/HB_CONTROLLER/HyperBUS/
## https://github.com/pulp-platform/tech_cells_generic.git
PULP_TECH_CELLS_HOME 	= /home/richy/projects/pulp_tech_cells_generic/tech_cells_generic
##https://github.com/pulp-platform/common_cells.git
PULP_COMMON_CELLS_HOME  = /home/richy/projects/pulp_tech_cells_generic/common_cells
##https://github.com/pulp-platform/axi_slice_dc.git
AXI_SLICE_HOME		=/home/richy/projects/pulp_tech_cells_generic/axi_slice_dc


#CELLS_SRC		:= $(PULP_TECH_CELLS_HOME)/src/deprecated/pulp_clock_gating_async.sv 
#			    $(PULP_TECH_CELLS_HOME)/src/deprecated/pulp_clk_cells.sv 
CELLS_SRC		:= -v $(PULP_TECH_CELLS_HOME)/src/deprecated/pulp_clk_cells.sv \
			   -y $(PULP_TECH_CELLS_HOME)/src/deprecated/ \
			   -y $(PULP_COMMON_CELLS_HOME)/src/deprecated/ \
			   -y $(PULP_COMMON_CELLS_HOME)/src/ \
			   $(AXI_SLICE_HOME)/src/dc_data_buffer.sv \
			   $(AXI_SLICE_HOME)/src/dc_token_ring_fifo_din.v \
			   $(AXI_SLICE_HOME)/src/dc_token_ring_fifo_dout.v \
			   -y $(AXI_SLICE_HOME)/src
RTLSRC			:= ../rtl/core/udma_ch_addrgen.sv \
    				../rtl/core/udma_arbiter.sv \
   				../rtl/core/udma_core.sv \
    				../rtl/core/udma_rx_channels.sv \
				../rtl/core/udma_tx_channels.sv \
			    ../rtl/core/udma_stream_unit.sv \
			    ../rtl/common/udma_ctrl.sv \
			    ../rtl/common/udma_apb_if.sv \
			    ../rtl/common/io_clk_gen.sv \
			    ../rtl/common/io_event_counter.sv \
			    ../rtl/common/io_generic_fifo.sv \
			    ../rtl/common/io_tx_fifo.sv \
			    ../rtl/common/io_tx_fifo_mark.sv \
			    ../rtl/common/io_tx_fifo_dc.sv \
			    ../rtl/common/io_shiftreg.sv \
			    ../rtl/common/udma_dc_fifo.sv \
			    ../rtl/common/udma_clkgen.sv \
			    ../rtl/common/udma_clk_div_cnt.sv

HBUS_SRC 		:=     $(HBUS_HOME)/rtl/hyperbus_clk_gen.sv \
			    $(HBUS_HOME)/rtl/hyperbus_cmd_addr_gen.sv \
			    $(HBUS_HOME)/rtl/hyperbus_ddr_in.sv \
			    $(HBUS_HOME)/rtl/hyperbus_ddr_out.sv \
			    $(HBUS_HOME)/rtl/hyperbus_phy.sv \
			    $(HBUS_HOME)/rtl/hyperbus_ctrl.sv \
			    $(HBUS_HOME)/rtl/hyperbus_read_clk_rwds.sv \
			    $(HBUS_HOME)/rtl/hyperbus_reg_if.sv \
			    $(HBUS_HOME)/rtl/hyperbus_delay_line.sv \
			    $(HBUS_HOME)/rtl/hyperbus_clock_diff_out.sv \
			    $(HBUS_HOME)/rtl/udma_hyperbus.sv

HPFLASH_SRC		:= $(HPFLASH_HOME)/sim/model/s26ks512s.sv \
			   $(HPFLASH_HOME)/sim/hbc_io.v			    	

vcs-sv-build:
		 vlogan +vc -sverilog -assert svaext -timescale=1ps/1ps  -f src_ips_files.f -f src_rtl_files.f -l debug.log +vc -sverilog -race=all -ignore unique_checks -full64 -kdb $(HBUS_SRC) $(HPFLASH_SRC) 

vcs-vhdl-build:
		vhdlan -full64 -f vhd.f -kdb

vcs-elab: 
	vcs -full64 -top tb_pulp \
	-pvalue+tb_pulp.USE_SDVT_SPI=0 -pvalue+tb_pulp.USE_SDVT_CPI=0 \
	-timescale=1ps/1ps \
	-pvalue+tb_pulp.BAUDRATE=115200 -pvalue+tb_pulp.ENABLE_DEV_DPI=0 \
	-pvalue+tb_pulp.LOAD_L2='JTAG' -pvalue+tb_pulp.USE_SDVT_I2S=0 \
	-pvalue+tb_pulp.USE_PULP_BUS_ACCESS=1 \
	-l vcs_compile.log -fsdb -debug_access+all -kdb -lca \
	-assert svaext -fsdb -full64 -cpp g++-4.8 -cc gcc-4.8

vcs-run:
		./simv $(SIMV_FLAGS) +fsdb+delta +fsdb+skip_cell_mode=2 +fsdb+all +fsdb+mda +fsdb+struct -l sim.log

vcs-run-full: vcs-sv-build vcs-vhdl-build vcs-elab vcs-run
