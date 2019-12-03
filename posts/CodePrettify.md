如何在 jupyter notebook 中启用 Code prettify 

```bash
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user

pip install jupyter_nbextensions_configurator
jupyter nbextensions_configurator enable --user

pip install yapf
```

然后在jupyter notebook 打开的页面中选 Nbextensions，启用  Code prettify 即可。
