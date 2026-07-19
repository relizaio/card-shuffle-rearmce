FROM node:24.14-trixie-slim@sha256:c319bb4fac67c01ced508b67193a0397e02d37555d8f9b72958649efd302b7f8 AS build-stage
ARG CI_ENV=noci
ARG GIT_COMMIT=git_commit_undefined
ARG GIT_BRANCH=git_branch_undefined
ARG VERSION=not_versioned
WORKDIR /app
COPY package*.json ./
RUN npm ci --ignore-scripts
COPY ./ .
RUN echo "version=$VERSION" > /app/version && echo "commit=$GIT_COMMIT" >> /app/version && echo "branch=$GIT_BRANCH" >> /app/version

# Distroless runtime: no shell, no npm/yarn/corepack in any layer, so
# all-layers SBOM scans see only the node runtime and the app tree.
FROM gcr.io/distroless/nodejs24-debian12:nonroot@sha256:14d42e2511532589a7c7e01a753667a74fcc96266e137e8125006b87b0c32d0a AS runtime-stage
ARG CI_ENV=noci
ARG GIT_COMMIT=git_commit_undefined
ARG GIT_BRANCH=git_branch_undefined
ARG VERSION=not_versioned
COPY --from=build-stage /app /app
WORKDIR /app

LABEL git_commit $GIT_COMMIT
LABEL git_branch $GIT_BRANCH
LABEL ci_environment $CI_ENV
LABEL version $VERSION

EXPOSE 3000

CMD ["index.js"]
