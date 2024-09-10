Which Dockerfile instruction adds files from the host to the container?

ADD and COPY docker instructions add or copy file(s) to the container. However, there are some differences: the ADD instruction only copies files from the local host to the container while the ADD instruction copies files from local host and from URL(fetches files from the internet) also the ADD instruction can decompress file while copying it inside the container.

Best approaches regarding coping and fetching files:
[1] If you only want to copy files, use the COPY instruction.

[2] If you want to to copy compressed file (.tar), use the ADD instruction.

[3] If you want to fetch the file from the internet, use the wget command.