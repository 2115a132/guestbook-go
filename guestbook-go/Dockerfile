# Copyright 2016 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.


FROM golang:1.10.0 AS builder

# Install dependencies
RUN go get github.com/codegangsta/negroni \
           github.com/gorilla/mux \
           github.com/xyproto/simpleredis/v2

# Set the working directory inside the container
WORKDIR /app

# Add the Go source code to the working directory
ADD ./main.go .

# Build the Go application with specific build flags
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Stage 2: Create the final lightweight image
FROM scratch

# Set the working directory inside the container
WORKDIR /app

# Copy the compiled Go binary from the builder stage
COPY --from=builder /app/main .

# Copy the public directory with all its contents
COPY ./guestbook-go/public/style.css public/style.css
COPY ./guestbook-go/public/index.html public/index.html
COPY ./guestbook-go/public/script.js public/script.js

# Set the command to run the Go application
CMD ["/app/main"]

# Expose port 3000 to allow external connections
EXPOSE 3000

