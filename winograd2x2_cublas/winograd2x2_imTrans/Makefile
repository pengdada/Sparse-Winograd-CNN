HOME=$(shell echo ~)
TF=$(shell python -c 'import tensorflow as tf; print(tf.__path__[1])')
TF_INC=$(TF)/include
LD_LIBRARY_PATH=$(HOME)/local/cuda-10.0/lib64
TF_LIB=$(TF)
CU_LIBRARY_PATH=$(HOME)/local/cuda-10.0/targets/x86_64-linux/lib


all: winograd2x2_imTrans_op.so winograd2x2_imTrans_grad_op.so

forward: winograd2x2_imTrans_op.so

backward: winograd2x2_imTrans_grad_op.so

winograd2x2_imTrans_op.so: winograd2x2_imTrans_op.cc winograd2x2_imTrans_cuda.cu.o
	g++ -std=c++11 -shared -o winograd2x2_imTrans_op.so winograd2x2_imTrans_op.cc winograd2x2_imTrans_cuda.cu.o -I $(TF_INC) -fPIC -L$(CU_LIBRARY_PATH) -L$(LD_LIBRARY_PATH) -lcudart -O2 -L$(TF_LIB) -ltensorflow_framework -D_GLIBCXX_USE_CXX11_ABI=0

winograd2x2_imTrans_cuda.cu.o: winograd2x2_imTrans_cuda.cu.cc
	nvcc -std=c++11 -c -o winograd2x2_imTrans_cuda.cu.o winograd2x2_imTrans_cuda.cu.cc -I $(TF_INC) -D GOOGLE_CUDA=1 -x cu -Xcompiler -fPIC -arch sm_52 -L$(TF_LIB) -ltensorflow_framework -D_GLIBCXX_USE_CXX11_ABI=0 # --use_fast_math
		
winograd2x2_imTrans_grad_op.so: winograd2x2_imTrans_grad_op.cc winograd2x2_imTrans_grad_cuda.cu.o
	g++ -std=c++11 -shared -o winograd2x2_imTrans_grad_op.so winograd2x2_imTrans_grad_op.cc winograd2x2_imTrans_grad_cuda.cu.o -I $(TF_INC) -fPIC -L$(CU_LIBRARY_PATH) -L$(LD_LIBRARY_PATH) -lcudart -O2 -L$(TF_LIB) -ltensorflow_framework -D_GLIBCXX_USE_CXX11_ABI=0

winograd2x2_imTrans_grad_cuda.cu.o: winograd2x2_imTrans_grad_cuda.cu.cc
	nvcc -std=c++11 -c -o winograd2x2_imTrans_grad_cuda.cu.o winograd2x2_imTrans_grad_cuda.cu.cc -I $(TF_INC) -D GOOGLE_CUDA=1 -x cu -Xcompiler -fPIC -arch sm_52 -L$(TF_LIB) -ltensorflow_framework -D_GLIBCXX_USE_CXX11_ABI=0 # --use_fast_math
	
clean:
	rm *.o *.so
