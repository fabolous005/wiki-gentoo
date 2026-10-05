<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Refine_and_complete_ROCm:_eclass,_more_packages_and_downstream_softwares | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2022/Ideas/Refine and complete ROCm: eclass, more packages and downstream softwares -->
---
title: "Google Summer of Code/2022/Ideas/Refine and complete ROCm: eclass, more packages and downstream softwares"
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Refine_and_complete_ROCm:_eclass,_more_packages_and_downstream_softwares
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-02"
fingerprint: c763536b881da513
license: CC BY-SA 4.0
---

# Google Summer of Code/2022/Ideas/Refine and complete ROCm: eclass, more packages and downstream softwares

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

ROCm™ open software platform is a open source software for HPC/Hyperscale-class GPU computing developed by AMD\[1\]. It currently support various AMD GPUs (also Nvidia GPUs, by wrapping CUDA), and may include more hardware like FPGA in the future. Packages can be classified to 4 categories: low-level drivers and runtime libraries, developer toolkit, high level libs and frameworks. Thanks to the contributor from ROCm overlay\[2\], Gentoo has packaged the most important ones.

However there are still a lot to be done:

1\. Enable ROCm for packages like tensorflow, jax, cupy;
2. Write a rocm.eclass to make ROCm related packages more maintainable\[3\], and consider USE Flag for different GPU architecture;
3. Enable more testing;
4. Hold a discussion about open source, heterogeneous computing platform and GNU/Linux distos. Due to it's open-source nature, ROCm packages can be carefully treated to meet FHS standard. But it contains binary kernels for GPU, which is not well considered\[4\], and testing GPU libraries require specific hardware\[5\]. Those are the challenges we must face if distros package heterogeneous compute packages;
5. More packages missing in ::gentoo, such as ROCgdb, rocWMMA, etc.
6. Current ebuild maintenance, including bug fix, stabilization.
7. Wiki page for ROCm usage and development.

References:

\[1\] [https://rocmdocs.amd.com/en/latest/index.html](https://rocmdocs.amd.com/en/latest/index.html)
\[2\] [https://github.com/justxi/rocm](https://github.com/justxi/rocm)
\[3\] [https://bugs.gentoo.org/810619](https://bugs.gentoo.org/810619)
\[4\] [https://bugs.gentoo.org/795825](https://bugs.gentoo.org/795825)
\[5\] [https://bugs.gentoo.org/817440](https://bugs.gentoo.org/817440)




| Contacts | Required Skills | 
|---|---|
| [Benda Xu](mailto:heroxbd@gentoo.org) |  | 
| Expected Project Size | Expected Outcomes | 
| 200 to 300 hours, depend on the actual plan. |  | 
| Project Difficulty |  | 
|  |  |
