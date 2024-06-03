# Simulator for GPU with Heterogeneous Memory Stack (HMS) 

This repository contains the source code of modified Accel-sim used in the following paper, which proposes the Heterogeneous Memory Stack (HMS).

Jeongmin Hong, Sungjun Cho, Geonwoo Park, Wonhyuk Yang, Young-Ho Gong and Gwangsun Kim, "Bandwidth-Effective DRAM Cache for GPU s with Storage-Class Memory,", HPCA'24.  
Arxiv link: https://arxiv.org/abs/2403.09358  
IEEE xplore: https://ieeexplore.ieee.org/document/10476449


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

The binary file of accel-sim will be generated as gpu-simulator/bin/release/accel-sim.out.

# How to run

The simulator requires three to run the simulation:

gpgpu-sim/run_simulator.sh file is the script to run the simulation.
This file automatically generates environment directories and files for the simulation.

The simulator supports the following memory designs:
- HMS
- HBM
- PCM
- HMS-BP (HMS without our two-level bypass policy)
- HMS-BP-CTC (HMS-BP without the Configurable Tag Cache (CTC))

Above options can be set in the run_simulator.sh file.
Below is the line to configure the memory design and its options:
```
export TARGET_MEM=HMS
```

To adjust the relative capacity of HBM compared to the memory footprint (R_HBM), modify the following line of the run_simulator.sh file:
```
export TARGET_FOOTPRINT_RATIO=75  # 75% of the workload's memory footprint can be in HBM (R_HBM = 75%)
```

The directory containing the trace of the target application should be specified in the run_simulator.sh file:
```
export TARGET_TRACE_DIR=$ACCELSIM_ROOT_DIR/gpgpu-sim/example_traces/color_maxmin_example/traces
```
In this repository, we provide only one example trace from kernel-1 of coloring_maxmin workload in the Pannotia benchmark suite (available at https://github.com/accel-sim/gpu-app-collection/tree/release).
Trace files for other kernels can be generated using the tracer for Accel-sim (see https://accel-sim.github.io/).

To generate the `num_pages.txt` file, which contains the number of pages for the target application, use the `generate_pagetable.ipynb` notebook. This notebook generates the `num_pages.txt` file from the trace files. Set the `target_traces_dir` variable in the `generate_pagetable.ipynb` notebook to the directory containing the trace files of the target application.

Finally, you can run the simulation as follows:
```
cd gpgpu-sim

sh run_simulator.sh
```

# Citation

Please cite our paper if you use our code for your work:

Bibtex entry:
 ```
@INPROCEEDINGS{10476449,
  author={Hong, Jeongmin and Cho, Sungjun and Park, Geonwoo and Yang, Wonhyuk and Gong, Young-Ho and Kim, Gwangsun},
  booktitle={2024 IEEE International Symposium on High-Performance Computer Architecture (HPCA)}, 
  title={Bandwidth-Effective DRAM Cache for GPUs with Storage-Class Memory}, 
  year={2024},
  volume={},
  number={},
  pages={139-155},
  doi={10.1109/HPCA57654.2024.00021}}
 ```
