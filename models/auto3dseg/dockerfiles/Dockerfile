FROM mhubai/base:latest

  
# load prerequisites 
RUN echo "loading prerequisite libraries"
RUN pip3 install  "monai-weekly[nibabel,tqdm]"
RUN pip install pynrrd
 
# Import the MHub model definiton
# *** hardcoding for now
ARG MHUB_MODELS_REPO="https://github.com/curtislisle/mhub_models"
RUN buildutils/import_mhub_model.sh auto3dseg ${MHUB_MODELS_REPO}

# pull the weights for the segresnet model trained on the abdominal total
# segmentator data model from the MONAIAuto3DSeg Slicer extension repository
ENV WEIGHTS_DIR="/root/.cache/torch/hub/checkpoints/"
ENV MODEL_NAME="abdominal-organs-3mm-v2.0.0.zip"
ENV WEIGHTS_URL_PREFIX="https://github.com/lassoan/SlicerMONAIAuto3DSeg/releases/download/Models/"
ENV WEIGHTS_URL=${WEIGHTS_URL_PREFIX}\/${MODEL_NAME}
RUN echo "weights:" ${WEIGHTS_URL}

# download the weights
# *** NOTE: this should be replaced with a call to buildutils/download_weights.sh
RUN wget --directory-prefix ${WEIGHTS_DIR} ${WEIGHTS_URL}
# the weights come bundled in a zip file, so unzip
ENV WEIGHTS_ARCHIVE = ${WEIGHTS_DIR}\/${MODEL_NAME}
ENV CURRENT_DIR=`pwd`
WORKDIR ${WEIGHTS_DIR}
RUN unzip ${MODEL_NAME}


# Default run script
#ENTRYPOINT ["python3", "-m", "mhubio.run"]
#CMD ["--config", "/app/models/auto3dseg/config/default.yml"]
ENTRYPOINT ["/bin/bash"]