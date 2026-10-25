# Testy cocotb dla SystolicTableAcceleratorWeightStationary
# uzycie: make <cel> [SIM=verilator|icarus]    (make help - lista celow)

ROOT       := $(abspath ../../..)
PYTHON_BIN ?= $(ROOT)/.venv/bin/python
VENV_DIR   := $(abspath $(dir $(PYTHON_BIN))/..)
SIM        ?= verilator

SV_DIR := $(abspath ..)/src

.PHONY: help test systolic_array compute_core accelerator_top clean

help:
	@echo "cele: test systolic_array compute_core accelerator_top clean"

test: systolic_array compute_core accelerator_top

systolic_array:
	bash -c '\
		unset TEST SIM_BUILD VERILOG_SOURCES TOPLEVEL MODULE PARAMS EXTRA_ARGS COMPILE_ARGS; \
		source $(VENV_DIR)/bin/activate && \
		$(MAKE) -f $(ROOT)/Makefile.test \
			SIM=$(SIM) \
			PROJECT_DIR=$(abspath ..) \
			TOPLEVEL=SystolicTableWeightStationary \
			MODULE=tests.SystolicArrayTest \
			SIM_BUILD=sim_build_systolic_array \
			VERILOG_SOURCES="$(SV_DIR)/pe/ProcessingElementWeightStationary.sv $(SV_DIR)/SystolicTableWeightStationary.sv" \
			PARAMS="ROWS=3 COLS=4"'

compute_core:
	bash -c '\
		unset TEST SIM_BUILD VERILOG_SOURCES TOPLEVEL MODULE PARAMS EXTRA_ARGS COMPILE_ARGS; \
		source $(VENV_DIR)/bin/activate && \
		$(MAKE) -f $(ROOT)/Makefile.test \
			SIM=$(SIM) \
			PROJECT_DIR=$(abspath ..) \
			TOPLEVEL=ComputeCore \
			MODULE=tests.ComputeCoreTest \
			SIM_BUILD=sim_build_compute_core \
			VERILOG_SOURCES="$(SV_DIR)/DataDelay.sv $(SV_DIR)/pe/ProcessingElementWeightStationary.sv $(SV_DIR)/SystolicTableWeightStationary.sv $(SV_DIR)/ComputeCore.sv" \
			PARAMS="ROWS=3 COLS=4"'

accelerator_top:
	bash -c '\
		unset TEST SIM_BUILD VERILOG_SOURCES TOPLEVEL MODULE PARAMS EXTRA_ARGS COMPILE_ARGS; \
		source $(VENV_DIR)/bin/activate && \
		$(MAKE) -f $(ROOT)/Makefile.test \
			SIM=$(SIM) \
			PROJECT_DIR=$(abspath ..) \
			TOPLEVEL=AcceleratorTop \
			MODULE=tests.AcceleratorTopTest \
			SIM_BUILD=sim_build_accelerator_top \
			VERILOG_SOURCES="$(SV_DIR)/DataDelay.sv $(SV_DIR)/pe/ProcessingElementWeightStationary.sv $(SV_DIR)/SystolicTableWeightStationary.sv $(SV_DIR)/ComputeCore.sv $(SV_DIR)/MemoryBuffer.sv $(SV_DIR)/AxiStreamSlave.sv $(SV_DIR)/AxiStreamMaster.sv $(SV_DIR)/AcceleratorFSM.sv $(SV_DIR)/AcceleratorTop.sv" \
			PARAMS="ROWS=3 COLS=4 ADDR_WIDTH=5 MEM_BACKEND=0"'

clean:
	rm -rf sim_build_* results.xml dump.vcd __pycache__
