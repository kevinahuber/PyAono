# PyAono
A Python implementation of the homomorphic encryption scheme by Yoshinori Aono et al. 

This scheme was first introduced in the paper - <a href="https://pdfs.semanticscholar.org/73f0/aa4e1b47b55f0f3d8464f61750e559067c56.pdf">"Fast and Secure Linear Regression and Biometric Authentication with Security Update"</a> by Yoshinori Aono et al. A really unique feature supported by this proposal is the support for key switching. All the code is written in <a href="http://pari.math.u-bordeaux.fr/">PARI library</a> in C++.

This code also has an independent header containing a PARI implementation of the Knuth-Yao's Algorithm for sampling from a discrete Gaussian distribution. The paper followed for understanding and implementing the algorithm - <a href="https://www.esat.kuleuven.be/cosic/publications/article-2372.pdf">High Precision Discrete Gaussian Sampling on FPGAs</a>.

NOTE : Running the homomorphic multiplication gives a ![equation](http://latex.codecogs.com/gif.latex?%24l*l%24) matrix as opposed to the message being ![equation](http://latex.codecogs.com/gif.latex?%241*l%24). This is because after homomorphic multiplication, we don't get message but we get ![equation](http://latex.codecogs.com/gif.latex?%24m%5ET*m%24).

--------

### News
New here? Check out our <a href="https://www.youtube.com/watch?v=jKKafEBx5P4">sample tutorial</a> on YouTube recorded by <a href="https://github.com/mayank0403">Mayank</a>. The tutorial covers a basic overview of how to use the API. More in-depth tutorials coming soon. Keep checking out YouTube <a href="https://www.youtube.com/channel/UCzoUqDE_OzYo6lGXtsEbOxQ">channel</a> and our GitHub repositories.

### Contribute
Welcome! If you are intrigued by our work and want to contribute, then please go through the tutorial as mentioned in the `News` section and contact any of the contributors. You should also go through this <a href="https://www.youtube.com/watch?v=sXFmKquiVnk">introductory video</a> of the OpenMined platform.

--------

### Importing API
The API can be imported using the command ```import Aono```. It currently supports the following functions and classes:

--------

## Functions Supported:
### 1. ```pari_init(pari_size, max_prime)```
```pari_init()``` is the function that needs to be called before dealing with this API. `pari_size` defines the size of stack we'll be using, and `max_prime` defines the pre computed prime table. 
#### Arguments: ```pari_size (int)```, ```max_prime (int)```
### 2. ```pari_close()```
```pari_close()``` function has to be called at the end of each program to clear the memory used.
### 3. ```create_GEN(x)```
```create_GEN()``` function converts integer `x` to `GEN`.
### 4. ```get_element(x, i)```
```get_element()``` function returns the `i^th` element of `GEN` variable `x`.  Index `i` is used to access the different components with indexing starting from `0`.
#### Arguments: ```x (GEN)```, ```i (int)```
### 5. ```print_GEN(x)```
```print_GEN()``` function prints the `GEN` variable `x`.
### 6. ```create_message_matrix(*x / x, l)```
```create_message_matrix(*x / x, l)``` function is used to make a PARI `t_MATRIX` object, which is just a conventional matrix, having a dimension of ```1*l``` (a row matrix) with the elements same as the input vector `x`. If input is just a number, then the first element of the row matrix is set as `x` and rest as `0`.
#### Arguments: ```input (int* or int)```, ```l (int)```
### 7. ```see_ciphertext(c, i)```
```see_ciphertext(c, i)``` function is used to access the contents of a ciphertext (type `struct cipher_text`). Since the ciphertext has 2 components, index `i` is used to decide which component to access, with `0` for component 1 and component 2 otherwise.
#### Arguments: ```c (cipher_text*)```, ```i (int)```

### 8. ```key_rotate(…)```
This function is yet to be implemented. Coming soon.
#### Arguments: Not yet decided

--------

## Classes:

### ```parameters```
  * Class Data:
    1. `n`, `s`, `sigma`, `l`, `lambda` (`ints`)
    2. `q`, `p` (`GEN`)

### ```secret_key```
  * Class Data:
    1. `sk` (`GEN`)
    2. `params` (`parameters*`)

  * ```__init__(self, sk = None, parmas = None)```   
    The constructor initiates class data. 
    Arguments: `sk` (`GEN`), `params` (`parameters*`)

  * ```decrypt(self, ct)```   
    ```decrypt()``` method returns the plaintext (`GEN`) encrypted in ciphertext `ct`.    Arguments: `ct` (`GEN`) 

  * ```serialize(self)```   
    TO BE IMPLEMENTED

### ```public_key```
  * Class Data:
    1. `pk` (`public_key_pack*`)
    2. `params` (`parameters*`)
    3. `g` (`globalvars*`)

  * ```__init__(self, pk = None, params = None, g = None)``` 
    The constructor initiates the class data. 
    Arguments: `pk` (`public_key_pack*`), `params` (`parameters*`), `g` (`globalvars*`)

  * ```encrypt(self, pt)```   
    ```encrypt()``` method returns the ciphertext (`GEN`) which encrypts plaintext `pt`. 
    Arguments: `pt` (`GEN`)

  * ```serialize(self)```   
    TO BE IMPLEMENTED

### ```key_pair```
  * Class Data:
    1. `sk` (`secret_key`)
    2. `pk` (`public_key`)

### ```key_gen```
  * ```generate_key(self, lambda, l, n, s, sigma, degree_p)```
    ```generate_key()``` method returns the keys, which is of type `key_pair`. Here `s` defines the tailprune and `degree_p` defines the `bit_size` of `p` to be generated. Arguments: `lambda` (`int`), `l` (`int`), `n` (`int`), `s` (`int`), `sigma` (`int`), `degree_p` (`int`).

  * ```deserialize(self)```   
    TO BE IMPLEMENTED

### ```ciphertext```   
    The class is compatible with `’+’, '*', and '-' operators`
  * Class Data:
    1. `value` (`GEN`)
    2. `pk` (`public_key*`)
    3. `params` (`parameters*`)

  * ```__init__(self, plaintext = None, pk, params)```  
    The constuctor method takes two arguments: `plaintext` (`GEN` variable), `pk` (`public_key*`), `params` (`parameters*`)

TODO - Remove `params`, it can be taken from `pk`.

  * ```decrypt(self, sk)```  
    ```decrypt()``` method returns the decrypted `ciphertext` which is `GEN` variable. 
Arguments: `sk` (`secret_key*`)



