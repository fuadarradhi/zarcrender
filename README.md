# zarcrender
Python JSON serverside render for any JS datagrid library

```python
def jsondata(request):

    out = {}
    arc = ARCJsonRender({
        'request': request.GET,
        'cursor': connection.cursor(),
        'dbms': dbms.SQL,
        'grid': grid.DATATABLES,
        'keys': keys.QUERYDICT
    })

    # arc.update_config({
    #     'request': request.GET,
    #     'cursor': connection.cursor(),
    #     'dbms': dbms.SQL,
    #     'grid': grid.DATATABLES,
    #     'keys': keys.QUERYDICT
    # })

    arc.set_query(" \
        SELECT * FROM usr_modul_action ma \
        __where__ __order__ __limit_offset__\
        ")
    # arc.set_query("SELECT * FROM usr_modul_action ma")

    def render_nomor(row, line):
        return '{}.'.format(line)

    def render_modul(row, line):
        return '<a href="{}">{}</a>'.format(row.uuid, row.modul)

    def render_action(row, line):
        return row.action

    def render_tombol(row, line):
        return '<a href="{}" class="btn btn-primary btn-md">Ubah</a>'.format(line)

    arc.add_column('nomor', render=render_nomor)
    arc.add_column('uuid', field='ma.uuid', filter=('ma.modul', 'ma.action'))
    arc.add_column('modul', field='ma.modul', render=render_modul)
    arc.add_column('action', render=render_action)
    arc.add_column('tombol', render=render_tombol)

    arc.get_json(out, dumps=False, key=False)

    return JsonResponse(out)
```
