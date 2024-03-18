# [HPCA'24] Bandwidth-Effective DRAM Cache for GPUs with Storage-Class Memory


# How to install

Tested environment: CentOS 7, CUDA 10.1, GCC 7.3.0

1. Set the environment variable, CUDA_INSTALL_PATH to the root directory of CUDA.
    ```
    export CUDA_INSTALL_PATH={path to CUDA}/cuda-10.1
    ```
  - If you do not set CUDA_INSTALL_PATH, you'll see the compilation error related to `vector_types.h` later.
  
2. Source the `setup_environment.sh` located in the directory, gpu-simulator. 
    ```
    cd gpu-simulator
    source setup_environment.sh
    ```
  - This will automatically clone the `gpgpusim_HMS` repository from our git. 
  
3. Compile the source code
    ```
    cd gpu-simulator
    make clean && make -j31 2> error.txt
    ```

4. The binary file of accel-sim is generated (see gpu-simulator/bin/release/accel-sim.out).

# How to run

The simulator receives 3 files to run the simulation:

gpgpu-sim/run_simulator.sh file is the script to run the simulation.
This file automatically generates environment directories and files for the simulation.

The simulation supports the following memory designs:
- HMS
- HBM
- PCM
- HMS-BP (HMS without our two-level bypass policy)
- HMS-BP-CTC (HMS-BP without the Configurable Tag Cache (CTC))

Above options can be set in the run_simulator.sh file.
Below is the line to set the memory design and the options:
```
export TARGET_MEM=HMS
```

To adjust the relative capacity of HBM compared to the memory footprint (R_HBM), set the following line of the run_simulator.sh file:
```
export TARGET_FOOTPRINT_RATIO=75  # 75% of the workload's memory footprint can be in HBM (R_HBM = 75%)
```

The target application's trace directory should be set in the run_simulator.sh file:
```
export TARGET_TRACE_DIR=$ACCELSIM_ROOT_DIR/gpgpu-sim/example_traces/color_maxmin_example/traces
```
  This is an example of one kernel of the workload because of the large volume of the trace file. A trace file can be created using an Accel-Sim tracer.

Finally, run the simulation:
```
cd gpgpu-sim

sh run_simulator.sh
```

