FROM sagemath/sagemath:9.0

# RUN sage -pip install jupyterlab
RUN sage -pip install cryptography

WORKDIR /home/sage/notebooks

EXPOSE 8888
CMD ["jupyter", "notebook", "--port=8888", "--no-browser", "--ip=0.0.0.0"]