# Go-Faiss Installation Guide
This article primarily explains how to resolve the following issue:

    autotunefaiss/c_api/AutoTune_c.h: No such file or directory
    5 | #include <faiss/c_api/AutoTune_c.h>
      |            ^~~~~~~~~~~~~~~~~~~~~~~~~~

## 1. Standard Installation
Here's how to install go-faiss according to the instructions provided on the [official website](https://github.com/DataIntelligenceCrew/go-faiss/):

    git clone https://github.com/facebookresearch/faiss.git
    cd faiss
    cmake -B build -DFAISS_ENABLE_GPU=OFF -DFAISS_ENABLE_C_API=ON -DBUILD_SHARED_LIBS=ON .
    make -C build
    sudo make -C build install

After that, run the following command:

    sudo cp build/c_api/libfaiss_c.so /usr/local/lib

This will allow you to install the module in Go as follows:

    go get github.com/DataIntelligenceCrew/go-faiss

According to the official instructions, after completing these steps, you should be able to use Faiss in Go. If you can successfully run the official [example](https://github.com/DataIntelligenceCrew/go-faiss/tree/master/_example), there's no need to continue. However, if you encounter the issue mentioned earlier, you can try the following solutions.

## 2. Troubleshooting
Currently, there are two main solutions found online:
1. Set an environment variable:

        export CGO_CFLAGS="-I /usr/local/include"

   
2. If your system is macOS, you may need to run the following command (I haven't tried it but has received many upvotes in an issue):

        sudo cp build/c_api/libfaiss_c.dylib /usr/local/lib/libfaiss_c.dylib

Since my system is a Linux system, I only tried the first method, but still reported the same error. Later, I tried the following method:

1. Navigate to the `/usr/local/include/faiss/` directory and observe that the `AutoTune_c.h` file's path is not in `faiss/c_api/AutoTune_c.h` but rather in `faiss/c_api/c_api/AutoTune_c.h`, where there is an extra `c_api` nested folder.

2. Manually adjust the structure in the `faiss` folder to remove the intermediate `c_api` layer, resulting in the path of `AutoTune_c.h` becoming `faiss/c_api/AutoTune_c.h`.
    <details> <summary>Faiss文件的树状结构</summary>

            .
            ├── AutoTune.h
            ├── c_api
            │   ├── AutoTune_c.h
            │   ├── clone_index_c.h
            │   ├── Clustering_c.h
            │   ├── error_c.h
            │   ├── error_impl.h
            │   ├── faiss_c.h
            │   ├── gpu
            │   │   ├── DeviceUtils_c.h
            │   │   ├── GpuAutoTune_c.h
            │   │   ├── GpuClonerOptions_c.h
            │   │   ├── GpuIndex_c.h
            │   │   ├── GpuIndicesOptions_c.h
            │   │   ├── GpuResources_c.h
            │   │   ├── macros_impl.h
            │   │   └── StandardGpuResources_c.h
            │   ├── impl
            │   │   ├── AuxIndexStructures_c.h
            │   │   └── c_api
            │   │       ├── AutoTune_c.h
            │   │       ├── clone_index_c.h
            │   │       ├── Clustering_c.h
            │   │       ├── error_c.h
            │   │       ├── error_impl.h
            │   │       ├── faiss_c.h
            │   │       ├── gpu
            │   │       │   ├── DeviceUtils_c.h
            │   │       │   ├── GpuAutoTune_c.h
            │   │       │   ├── GpuClonerOptions_c.h
            │   │       │   ├── GpuIndex_c.h
            │   │       │   ├── GpuIndicesOptions_c.h
            │   │       │   ├── GpuResources_c.h
            │   │       │   ├── macros_impl.h
            │   │       │   └── StandardGpuResources_c.h
            │   │       ├── impl
            │   │       │   └── AuxIndexStructures_c.h
            │   │       ├── Index_c.h
            │   │       ├── index_factory_c.h
            │   │       ├── IndexFlat_c.h
            │   │       ├── index_io_c.h
            │   │       ├── IndexIVF_c.h
            │   │       ├── IndexIVFFlat_c.h
            │   │       ├── IndexLSH_c.h
            │   │       ├── IndexPreTransform_c.h
            │   │       ├── IndexShards_c.h
            │   │       ├── macros_impl.h
            │   │       └── MetaIndexes_c.h
            │   ├── Index_c.h
            │   ├── index_factory_c.h
            │   ├── IndexFlat_c.h
            │   ├── index_io_c.h
            │   ├── IndexIVF_c.h
            │   ├── IndexIVFFlat_c.h
            │   ├── IndexLSH_c.h
            │   ├── IndexPreTransform_c.h
            │   ├── IndexShards_c.h
            │   ├── macros_impl.h
            │   └── MetaIndexes_c.h
            ├── clone_index.h
            ├── Clustering.h
            ├── impl
            │   ├── AuxIndexStructures.h
            │   ├── FaissAssert.h
            │   ├── FaissException.h
            │   ├── HNSW.h
            │   ├── io.h
            │   ├── io_macros.h
            │   ├── lattice_Zn.h
            │   ├── NNDescent.h
            │   ├── NSG.h
            │   ├── platform_macros.h
            │   ├── PolysemousTraining.h
            │   ├── pq4_fast_scan.h
            │   ├── ProductQuantizer.h
            │   ├── ProductQuantizer-inl.h
            │   ├── ResultHandler.h
            │   ├── ScalarQuantizer.h
            │   ├── simd_result_handlers.h
            │   ├── ThreadedIndex.h
            │   └── ThreadedIndex-inl.h
            ├── Index2Layer.h
            ├── IndexBinaryFlat.h
            ├── IndexBinaryFromFloat.h
            ├── IndexBinary.h
            ├── IndexBinaryHash.h
            ├── IndexBinaryHNSW.h
            ├── IndexBinaryIVF.h
            ├── index_factory.h
            ├── IndexFlat.h
            ├── Index.h
            ├── IndexHNSW.h
            ├── index_io.h
            ├── IndexIVFFlat.h
            ├── IndexIVF.h
            ├── IndexIVFPQFastScan.h
            ├── IndexIVFPQ.h
            ├── IndexIVFPQR.h
            ├── IndexIVFSpectralHash.h
            ├── IndexLattice.h
            ├── IndexLSH.h
            ├── IndexNNDescent.h
            ├── IndexNSG.h
            ├── IndexPQFastScan.h
            ├── IndexPQ.h
            ├── IndexPreTransform.h
            ├── IndexRefine.h
            ├── IndexReplicas.h
            ├── IndexScalarQuantizer.h
            ├── IndexShards.h
            ├── invlists
            │   ├── BlockInvertedLists.h
            │   ├── DirectMap.h
            │   ├── InvertedLists.h
            │   ├── InvertedListsIOHook.h
            │   └── OnDiskInvertedLists.h
            ├── IVFlib.h
            ├── MatrixStats.h
            ├── MetaIndexes.h
            ├── MetricType.h
            ├── utils
            │   ├── AlignedTable.h
            │   ├── distances.h
            │   ├── extra_distances.h
            │   ├── hamming.h
            │   ├── hamming-inl.h
            │   ├── Heap.h
            │   ├── ordered_key_value.h
            │   ├── partitioning.h
            │   ├── quantize_lut.h
            │   ├── random.h
            │   ├── simdlib_avx2.h
            │   ├── simdlib_emulated.h
            │   ├── simdlib.h
            │   ├── utils.h
            │   └── WorkerThread.h
            └── VectorTransform.h
    </details>

3. Run the [example](https://github.com/DataIntelligenceCrew/go-faiss/tree/master/_example) file again and observe that it now works successfully.

# Summary
This problem is not particularly challenging, but there's a scarcity of online tutorials, and I have been learning the go language recently. Therefore, this serves as a learning record.


> [https://github.com/facebookresearch/faiss](https://github.com/facebookresearch/faiss)
> [https://github.com/DataIntelligenceCrew/go-faiss](https://github.com/DataIntelligenceCrew/go-faiss)




