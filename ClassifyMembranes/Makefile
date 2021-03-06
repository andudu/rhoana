CFLAGS=-I$(OPENCV)/include -I$(HDF5_DIR)/include -g -O3
CXXFLAGS=-I$(OPENCV)/include -I$(HDF5_DIR)/include -g -O3
LDFLAGS=-L$(OPENCV)/lib -lopencv_highgui -lopencv_imgproc -lopencv_core -L$(HDF5_DIR)/lib -lhdf5_cpp -lhdf5

all: compute_features show_features test_image_to_hdf5 train_randomforest predict_randomforest train_gb predict_gb classify_image sketch_on_image

BLOSC_DIR=../blosc
BLOSC_SRCS=$(wildcard $(BLOSC_DIR)/blosc/*.c $(BLOSC_DIR)/hdf5/blosc_filter.c)
BLOSC_O=$(BLOSC_SRCS:.c=.o)

opencv_hdf5.o: opencv_hdf5.cpp
	g++ $(CXXFLAGS) -I$(BLOSC_DIR)/hdf5 -I$(BLOSC_DIR)/blosc -c $^

membranes.o: quickmedian.h

compute_features: opencv_hdf5.o $(BLOSC_O) compute_features.o membranes.o adapthisteq.o local_statistics.o tensor_gradient.o drawhist.o vesicles.o
	g++ -o $@ $^ $(LDFLAGS)

show_features: opencv_hdf5.o $(BLOSC_O) show_features.o
	g++ -o $@ $^ $(LDFLAGS)

train_randomforest: opencv_hdf5.o $(BLOSC_O) train_randomforest.o
	g++ -o $@ $^ $(LDFLAGS) -lopencv_ml

predict_randomforest: opencv_hdf5.o $(BLOSC_O) predict_randomforest.o
	g++ -o $@ $^ $(LDFLAGS) -lopencv_ml

train_gb: opencv_hdf5.o $(BLOSC_O) train_gb.o
	g++ -o $@ $^ $(LDFLAGS) -lopencv_ml

predict_gb: opencv_hdf5.o $(BLOSC_O) predict_gb.o
	g++ -o $@ $^ $(LDFLAGS) -lopencv_ml


test_image_to_hdf5: opencv_hdf5.o $(BLOSC_O) test_image_to_hdf5.o 
	g++ -o $@ $^ $(LDFLAGS)

sketch_on_image: sketch_on_image.o
	g++ -o $@ $^ $(LDFLAGS)

classify_image: opencv_hdf5.o $(BLOSC_O) classify_image.o membranes.o adapthisteq.o local_statistics.o tensor_gradient.o drawhist.o vesicles.o
	g++ -o $@ $^ $(LDFLAGS)

