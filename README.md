# Ares({A}PI {R}elated {E}rror {S}pecification Inference)
Ares: Inferring Error Specifications through Precise and Fast Semantic Analysis
>> THU, Ares Group, contact us at chi-li18@mails.tsinghua.edu.cn

### What is Ares?

An important category of API-related defects is error handling defects, which may result in security and reliability flaws. They can be detected under the help of static program analysis, provided that error specifications are known. Writing error specifications manually is time-consuming and tedious, therefore automatic inference from API usage patterns is preferred. 
We present Ares, a tool for automatically inferring precise error specifications for C language based on fast static analysis. Multiple heuristics are employed to identify error handling blocks and error specifications are inferred by analyzing error handling logic. 

Ares is evaluated on 19 real world projects, and the specifications inferred from Ares help identify dozens of API-related bugs in well-known projects such as OpenSSL, 10 of which are confirmed or fixed. We are trying our best to apply Ares to more programs. We upload the details in [evaluation_data/new_bugs](evaluation_data/new_bugs) 

Usage of our tools at [tools/Readme.md](tools/Readme.md)

Ares is still under development, and contains a lot of bugs and TODO lists. Any bugs or feature requests, feel free to email us at chi-li18@mails.tsinghua.edu.cn or open issues.

### What is in this repo?

- Evaluation data ([evaluation_data](evaluation_data))
  - 19 real world projects compiled IR Results
  - Ares evaluation result
  - APEx evaluation result
  - New bugs found by Ares
- IMSpec used in this repository ([imspec](imspec))
- Tools([tools](tools))
  - Ares tookit
  - build-capture



### Evaluation Data

We select 19 widely used projects, i.e., [keepalived](https://github.com/acassen/keepalived), [openssl](https://github.com/openssl/openssl) et al.  We evaluate our approach from two perspectives.

- Inferred error specifications. We employ 19 real world projects. For each project, Ares finds error specifications using multiple heuristics and the found error specifications are showed in the errspec.txt. These projects are used for comparison with state-of-the-art methods in terms of precision and recall. 
- New bugs in real world projects. We also apply Ares to the latest versions of real-world programs to evaluate whether our approach can find new bugs.

We also test these projects on  [APEx](https://github.com/yujokang/APEx), an automically error specification inference tool.

We upload the compiled IR Results of 19 projects at [evaluation_IR_Results](evaluation_data) and original results at [evaluation_data](evaluation_data).

#### New Bugs

The main motivation of Ares is to infer error specifications and using the specifications to detect API-misuse bugs in real-world programs, namely, to determine whether Ares can find error specifications and previously unkown bugs. Therefore, we apply Ares to the 19 projects to find error specifications. And using the inferred specifications, we apply the specifications to an API-misuse detecting tool and 10 error handling bugs are confirmed by developers.

Up to now, 10 previously unknown bugs have been confirmed by developers. 

|      Project      | Issue# | Error Specification
| :---------------: | -------------------------------------: |---------:
|      keepalived      |                             1003 | SSL\_CTX\_new, eq null
|      keepalived       |                             1004 | SSL\_new, eq null
|        httping        |                               41 | SSL\_CTX\_new, eq null
|       irssi        |                               943 | BIO\_read, slt 0
|      irssi      |                               944 | SSL\_get\_peer\_certificate, eq null
|      sslsplit      |                               224 | SSL\_CTX\_use\_certificate, eq 0
|     sslsplit     |                               225 | SSL\_CTX\_use\_PrivateKey, sle 0
|   thc-ipv6   |                               28 | BN\_new, eq null
|       openssl       |                               6567 | RAND\_bytes, sle 0
|     openssl     |                               6973 | EVP\_MD\_CTX\_new, eq null

We upload the details  in [evaluation_data/new_bugs](evaluation_data/new_bugs) 


### Tools

#### How to use

Ares can be used with the following steps: 

  - make sure that target project can be compiled by gcc, then using our build-capture tool to capture its build sequence automatically. The captured results are preprocessed by expanding the macros and in-lining header files. Then using the captured results, we can generate the corresponding IR results which are shown in [evaluation_IR_Results](evaluation_data).
  - Trigger the major work of error specification mining. It first parses IR results into CFA and CG, then performs static analysis. Inferred specifications are written to the errspec.txt file which are shown in each project of [evaluation_data](evaluation_data).

