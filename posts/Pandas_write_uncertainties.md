Pandas 输入含有非对称误差的字符串以及输出成 LaTex 表格.

Pandas, input string with asymmetric errors (asymmetric uncertainties) and output / export into LaTex table

```python
mass1_summary_data = pd.DataFrame()
append_dict={}
append_dict['Name'] = "Name"
append_dict[par_] = '$' + format(median, '0.2f') + "^{+" + format(plus, '0.2f') + "}" + "_{-" + format(minus, '0.2f') + "}"  + '$'
mass1_summary_data=mass1_summary_data.append(append_dict,ignore_index=True)
print(mass1_summary_data.to_latex(escape=False,index=False,na_rep="\\backslash"))
```
