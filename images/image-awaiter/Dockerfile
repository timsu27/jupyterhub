# compile the code to an executable using an intermediary image
FROM golang:1.17

# VULN_SCAN_TIME=

RUN mkdir -p /build/
COPY *.mod *.go *.sum /build/
WORKDIR /build
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags '-w -s' -installsuffix cgo -a -o out/image-awaiter


# present the result within a slimmed image
FROM scratch

COPY --from=0 /build/out/image-awaiter /image-awaiter



# To debug / develop this code
# ----------------------------
# 1. Setup a kubectl proxy
# > kubectl proxy --port=8080

# 2. Try the API using the proxy...
# > curl http://localhost:8080/apis/apps/v1/namespaces/<namespace>/demonsets/hook-image-puller

# 3. Try the container using the proxy...
# > docker build --tag <name:tag> .
# > docker run -it --rm --net=host <name:tag> /image-awaiter -debug -namespace <namespace> -daemonset hook-image-puller
