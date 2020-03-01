---
title: System information for better reproducibility in Python
author: Aritra Biswas
date: '2020-03-02'
categories:
  - Programming
  - Python
tags:
  - Best practices
  - Code Quality
  - Documentation
  - Python
cover: "/img/python_img/python_logo.png"
---

One of the essential aspects of coding is debugging. Reproducibility makes it easy to share a problem. I have personally seen that if we are stuck in a massive problem, making a smaller reproducible version of it helps in many ways. It is not always easy to think about the problem with a higher dimension of complexity, sometimes making it easier helps to understand the issue in a better way. 

<!--more-->

Many times I have seen that while formulating the problem, the solution comes to my mind. I don't know if the same for everyone else or not. But quite often, I have seen that people share a problem, which is perfectly reproducible, but the environment-related information is missing in the post or the log. It is tough and many times impossible to solve such a problem if the required information about the environment and libraries are not known. Here is a snippet that I use while logging or sharing any code with my peers, which is quite useful, in my opinion. 

```python
import pandas as pd
import numpy as np
from logzero import logger
import numpy as np
import numexpr
import platform
import socket
import psutil
import pyarrow as pa

def show_system_info(logger=logger):
    logger.info(f"Pandas version: {pd.__version__}")
    logger.info(f"Numpy version: {np.__version__}")
    logger.info(f"Number of CPU cores: {pa.cpu_count()}")
    logger.info(f"Platform: {platform.system()}")
    logger.info(f"Platform release: {platform.release()}")
    logger.info(f"Platform version: {platform.version()}")
    logger.info(f"Architecture: {platform.machine()}")
    logger.info(f'Hostname: {socket.gethostname()}')
    logger.info(f'IP address: {socket.gethostbyname(socket.gethostname())}')
    logger.info(f'RAM : {str(round(psutil.virtual_memory().total / (1024.0 **3)))+" GB"}')
    logger.info("Pandas, Numpy and  Numexpr metadata:\n")
    pd.show_versions()
    np.show_config()
    numexpr.print_versions()
show_system_info()
```

It not only helps to understand the about the system but it also gives a few information about the avaiable system libraries and hardware specification which is import in many case. Here is a sample output of the above mentioned function.


![](/post/2020-03-02-log-system-and-libraries-information-for-better-reproducibility-in-python_files/get_system_info_part1.jpg)

![](/post/2020-03-02-log-system-and-libraries-information-for-better-reproducibility-in-python_files/get_system_info_part2.jpg)

![](/post/2020-03-02-log-system-and-libraries-information-for-better-reproducibility-in-python_files/get_system_info_part3.jpg)

At times for profiling purpose it is important to know the backend for numpy. The above output shows various important information about the system libraries. I find it important and try to use it as much as possible in log and in the starting of any jupyter notebook. It can be made more informative and interpretable. In future I will try to come up with a better version of this function.