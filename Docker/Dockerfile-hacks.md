While reviewing Dockerfiles, I noticed not everyone uses the --no-install-recommends option. This flag is crucial as it prevents the installation of unnecessary packages, leading to smaller image sizes and faster build times.

Example:
RUN apt-get install -y --no-install-recommends <package>

By adopting this practice, you’ll keep your images lean and your builds efficient.

------------------