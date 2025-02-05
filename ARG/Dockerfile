ARG version
FROM almalinux:${version:-9}
ARG company="TCS" \
    project="Ericsson"
ENV duration=$duration
RUN echo "Company: $company, Project: $project, Version: $version"
CMD ["sleep","100"]