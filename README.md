[<img align="right" alt="Grid Smarter Cities" src="https://s3.eu-west-2.amazonaws.com/open-source-resources/grid_smarter_cities_small.png">](https://www.gridsmartercities.com/)

![Build Status](https://codebuild.eu-west-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiQjBXUGt0ZUFsYzJjMmlIclVRc2FvM3NmTm9pQVAyWlhKZmJVd09nQUQyMnZkTUhDaUVKSEVSb2VtMmNoaEVDTU5sRXdvVzFrQkhQdTljNWVzM2ZzUWxBPSIsIml2UGFyYW1ldGVyU3BlYyI6IjVnK0tZOXZCUXU3UXAyaXkiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Github Release](https://img.shields.io/github/release/gridsmartercities/aws-lambda-decorators.svg?style=flat)
\
\
![Python Versions](https://img.shields.io/pypi/pyversions/aws-lambda-decorators.svg?style=flat)
![PyPi Version](https://img.shields.io/pypi/v/aws-lambda-decorators.svg?style=flat)
![PyPi Downloads](https://img.shields.io/pypi/dm/aws-lambda-decorators.svg?style=flat)
![PyPi Status](https://img.shields.io/pypi/status/aws-lambda-decorators.svg?style=flat)

# aws-lambda-decorators

A set of Python decorators to ease the development of AWS lambda functions.

## Installation

The easiest way to use these AWS Lambda Decorators is to install them through Pip:

`pip install -i https://test.pypi.org/simple/ aws-lambda-decorators`

## Package Contents

### [Decorators](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py)

The current list of AWS Lambda Python Decorators includes:

* [__extract__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L56-L85): a decorator to extract and validate specific keys of a dictionary parameter passed to a AWS Lambda function.
* [__extract_from_event__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L24-L37): a facade of [__extract__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L56-L85) to extract and validate keys from an AWS API Gateway lambda function _event_ parameter.
* [__extract_from_context__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L40-L53): a facade of [__extract__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L56-L85) to extract and validate keys from an AWS API Gateway lambda function _context_ parameter.
* [__extract_from_ssm__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L130-L152): a decorator to extract from AWS SSM the values of a set of parameter keys.
* [__validate__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L177-L200): a decorator to validate a list of function parameters.
* [__log__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L116-L127): a decorator to log the parameters passed to the lambda function and/or the response of the lambda function.
* [__handle_exceptions__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L88-L113): a decorator to handle any type of declared exception generated by the lambda function. 
* [__response_body_as_json__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L155-L174): a decorator to transform the body of a response to json.

### [Validators](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/validators.py)

Currently, the package offers 2 validators:

* [__Mandatory__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/validators.py#L5-L16): Checks if a parameter has a not None value.
* [__RegexValidator__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/validators.py#L19-L38): Checks a parameter against a regular expression.

### [Decoders](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decoders.py)

The package offers functions to decode from JSON and JWT. 

* [__decode_json__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decoders.py#L44-L46): decodes/converts a json string to a python dictionary
* [__decode_jwt__](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decoders.py#L49-L51): decodes/converts a JWT string to a python dictionary

## Examples

### [extract](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L56-L85)

This decorator extracts and validates values from dictionary parameters passed to a Lambda Function.

* The decorator takes a list of [Parameter](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/classes.py#L109-L207) objects.
* Each [Parameter](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/classes.py#L109-L207) object requires a non-empty path to the parameter in the dictionary, and the name of the dictionary (func_param_name)
* The parameter value is extracted and added as a kwarg to the lambda handler (or any other decorated function/method).
* You can add the parameter to the handler signature, or access it in the handler through kwargs.
* The name of the extracted parameter is defaulted to the last element of the path name, but can be changed by passing a (valid pythonic variable name) var_name
* You can define a default value for the parameter in the [Parameter](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/classes.py#L109-L207) or in the lambda handler itself.
* A 400 exception is raised when the parameter cannot be extracted or when it does not validate.
* A variable path (e.g. '/headers/Authorization[jwt]/sub') can be annotated to specify a decoding. In the example, Authorization might contain a JWT, which needs to be decoded before accessing the "sub" element.

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L10-L24) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L11-L31)
```python
@extract(parameters=[
    Parameter(path='/parent/my_param', func_param_name='a_dictionary'),  # extracts a non mandatory my_param from a_dictionary
    Parameter(path='/parent/missing_non_mandatory', func_param_name='a_dictionary', default='I am missing'),  # extracts a non mandatory missing_non_mandatory from a_dictionary
    Parameter(path='/parent/missing_mandatory', func_param_name='a_dictionary'),  # does not fail as the parameter is not validated as mandatory
    Parameter(path='/parent/child/id', validators=[Mandatory], var_name='user_id', func_param_name='another_dictionary')  # extracts a mandatory id as "user_id" from another_dictionary
])
def extract_example(a_dictionary, another_dictionary, my_param='aDefaultValue', missing_non_mandatory='I am missing', missing_mandatory=None, user_id=None):
    """
        Given these two dictionaries:
        
        a_dictionary = { 
            'parent': { 
                'my_param': 'Hello!' 
            }, 
            'other': 'other value' 
        }
        
        another_dictionary = { 
            'parent': { 
                'child': { 
                    'id': '123' 
                } 
            } 
        }
    
        you can now access the extracted parameters directly: 
    """
    return my_param, missing_non_mandatory, missing_mandatory, user_id
```

Or you can use kwargs instead of specific parameter names:

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L27-L32) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L33-L45)
```python
@extract(parameters=[
    Parameter(path='/parent/my_param', func_param_name='a_dictionary')  # extracts a non mandatory my_param from a_dictionary
])
def extract_to_kwargs_example(a_dictionary, **kwargs):
    """
        a_dictionary = { 
            'parent': { 
                'my_param': 'Hello!' 
            }, 
            'other': 'other value' 
        }
    """
    return kwargs['my_param']  # returns 'Hello!'
```

A missing mandatory parameter, or a parameter that fails validation, will raise an exception:

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L35-L40) | [test 1](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L47-L59) | [test 2](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L61-L73)
```python
@extract(parameters=[
    Parameter(path='/parent/mandatory_param', func_param_name='a_dictionary', validators=[Mandatory])  # extracts a mandatory mandatory_param from a_dictionary
])
def extract_mandatory_param_example(a_dictionary, mandatory_param=None):
    return 'Here!'  # this part will never be reached, if the mandatory_param is missing
    
response = extract_mandatory_param_example({'parent': {'my_param': 'Hello!'}, 'other': 'other value'} )

print(response)  # prints { 'statusCode': 400, 'body': 'Error extracting parameters' } and logs a more detailed error

```

You can decode any part of the parameter path from json or any other existing annotation.

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L43-L48) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L75-L86)
```python
@extract(parameters=[
    Parameter(path='/parent[json]/my_param', func_param_name='a_dictionary')  # extracts a non mandatory my_param from a_dictionary
])
def extract_from_json_example(a_dictionary, my_param=None):
    """
        a_dictionary = { 
            'parent': '{"my_param": "Hello!" }', 
            'other': 'other value' 
        }
    """
    return my_param  # returns 'Hello!'

```

### [extract_from_event](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L24-L37)

This decorator is just a facade to the [extract](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L56-L85) method to be used in AWS Api Gateway Lambdas. It automatically extracts from the event lambda parameter.

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L51-L58) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L88-L103)
```python
@extract_from_event(parameters=[
    Parameter(path='/body[json]/my_param', validators=[Mandatory]),  # extracts a mandatory my_param from the json body of the event
    Parameter(path='/headers/Authorization[jwt]/sub', validators=[Mandatory], var_name='user_id')  # extract the mandatory sub value as user_id from the authorization JWT
])
def extract_from_event_example(event, context, my_param=None, user_id=None):
    """
        event = { 
            'body': '{"my_param": "Hello!"}', 
            'headers': { 
                'Authorization': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c' 
            } 
        }
    """
    return my_param, user_id  # returns ('Hello!', '1234567890')
```

### [extract_from_context](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L40-L53)

This decorator is just a facade to the [extract](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L56-L85) method to be used in AWS Api Gateway Lambdas. It automatically extracts from the context lambda parameter.

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L61-L66) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L105-L117)
```python
@extract_from_context(parameters=[
    Parameter(path='/parent/my_param', validators=[Mandatory])  # extracts a mandatory my_param from the parent element in context
])
def extract_from_context_example(event, context, my_param=None):
    """
        context = {
            'parent': {
                'my_param': 'Hello!'
            }
        }
    """    
    return my_param  # returns 'Hello!'
```

### [extract_from_ssm](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L130-L152)

This decorator extracts a parameter from AWS SSM and passes the parameter down to your function as a kwarg.

* The decorator takes a list of [SSMParameter](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/classes.py#L54-L70) objects.
* Each [SSMParameter](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/classes.py#L54-L70) object requires the name of the SSM parameter (ssm_name)
* If no var_name is passed in, the extracted value is passed to the function with the ssm_name name

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L69-L76) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L120-L139)
```python
@extract_from_ssm(ssm_parameters=[
    SSMParameter(ssm_name='one_key'),  # extracts the value of one_key from SSM as a kwarg named "one_key"
    SSMParameter(ssm_name='another_key', var_name="another")  # extracts another_key as a kwarg named "another"
])
def extract_from_ssm_example(your_func_params, one_key=None, another=None):
    return your_func_params, one_key, another
```

### [validate](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L177-L200)

This decorator validates a list of non dictionary parameters from your lambda function.

* The decorator takes a list of [ValidatedParameter](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/classes.py#L73-L106) objects.
* Each parameter object needs the name of the lambda function parameter that it is going to be validated, and the list of rules to validate.
* A 400 exception is raised when the parameter does not validate.

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L79-L86) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L141-L146)
```python
@validate(parameters=[
    ValidatedParameter(func_param_name='a_param', validators=[Mandatory]),  # validates a_param as mandatory
    ValidatedParameter(func_param_name='another_param', validators=[Mandatory, RegexValidator(r'\d+')])  # validates another_param as mandatory and containing only digits
])
def validate_example(a_param, another_param):
    return a_param, another_param  # returns 'Hello!', '123456
    
validate_example('Hello!', '123456')
```

Given the same function `validate_example`, a 400 exception is returned if at least one parameter does not validate:

```python
validate_example('Hello!', 'ABCD')  # returns a 400 status code and an error message
```

### [log](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L116-L127)

This decorator allows for logging the function arguments and/or the response.

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L89-L91) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L157-L165)
```python
@log(parameters=True, response=True)
def log_example(parameters): 
    return 'Done!'
    
log_example('Hello!')  # logs 'Hello!' and 'Done!'
```

### [handle_exceptions](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L88-L113)

This decorator handles a list of exceptions, returning a 400 response containing the specified friendly message to the caller.

* The decorator takes a list of [ExceptionHandler](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/classes.py#L11-L33) objects.
* Each [ExceptionHandler](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/classes.py#L11-L33) requires the type of exception to check, and the friendly message to return to the caller.

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L94-L101) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L168-L181)
```python
@handle_exceptions(handlers=[
    ExceptionHandler(ClientError, "Your message when a client error happens.")
])
def handle_exceptions_example():
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('non_existing_table')
    table.query(KeyConditionExpression=Key('user_id').eq(user_id))
    # ...
    
handle_exceptions_example()  # returns {'body': 'Your message when a client error happens.', 'statusCode': 400}
```

### [response_body_as_json](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/aws_lambda_decorators/decorators.py#L155-L174)

This decorator ensures that, if the response contains a body, the body is dumped as json.

Example: [code](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/examples.py#L104-L106) | [test](https://github.com/gridsmartercities/aws-lambda-decorators/blob/master/examples/test_examples.py#L183-L188)
```python
@response_body_as_json
def response_body_as_json_example():
    return {'statusCode': 400, 'body': {'param': 'hello!'}}
    
response_body_as_json_example()  # returns { 'statusCode': 400, 'body': "{ 'param': 'hello!' }" }
```

## Documentation

You can get the docstring help by running:  

```bash
>>> from aws_lambda_decorators.decorators import extract
>>> help(extract)
```

## Links

* [PyPi](https://test.pypi.org/project/aws-lambda-decorators/)
* [Github](https://github.com/gridsmartercities/aws-lambda-decorators)