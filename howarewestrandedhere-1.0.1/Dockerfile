FROM nfcore/base:1.14
LABEL authors="@BarryDigby" \
    description="Docker image for signalbash/how_are_we_stranded_here"

COPY environment.yml /

RUN conda env create --quiet -f /environment.yml && conda clean -a

ENV PATH /opt/conda/envs/howarewestrandedhere/bin:$PATH

RUN conda env export --name howarewestrandedhere > how_are_we_stranded_here.yml
