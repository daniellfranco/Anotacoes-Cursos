# MVC - Model View Controller

É um padrão que separa um aplicativo em 3 grupos de componentes principais:
Modelos, Exibições e Componentes. Esse padrão ajuda a obter a separação de interesses.

---

</br>

# Context, Controllers e Rotas das páginas

As classes Context do MVC são exatamente iguais as dos projetos webapi. [Ver Anotações WebAPIs](../5%20-%20APIs%20com%20C%23/Anota%C3%A7%C3%B5es%20WebAPIs.md).

As classes Controller do MVC herdam de Controller ao invés de ControllerBase, de resto é tudo igual as webapi. [Ver Anotações WebAPIs](../5%20-%20APIs%20com%20C%23/Anota%C3%A7%C3%B5es%20WebAPIs.md).
Obs: no MVC é opcional colocar tag HttpGet, mas as outras como HttpPost é necessário por.

O Controller das paginas é influenciado pelo nome, HomeController vai automaticamente buscar as views dos seus métodos na pasta Home (pois antes do Controller está escrito Home, se tivesse OlaController ele iria buscar na pasta Ola) que está na pasta Views, assim como o return View(); vai retornar a pagina com o nome correspondente ao método em que ele está, exemplo Index() ou Privacy().

![MarcacoesComoRotasFuncionam](imgs/MarcacoesComoRotasFuncionam.png)

Quando acessar o link da pagina referente a um controller, exemplo:  
`https://localhost:7277/Home`  
Automaticamente ele puxa o método Index.  
O Index é a única exceção, os outros é necessário dizer o caminho certinho, exemplo:  
`https://localhost:7277/Home/Privacy`

---

</br>

# Paginas

As paginas são montadas com html de forma similar a um site normal, e são salvas no formato `.cshtml`, elas tem que ficar dentro de uma pasta Views, e em seguida uma pasta que seja referente ao que fazem, por exemplo você vai criar uma pasta Contato para colocar paginas com assuntos referentes a contatos dentro.

O formato `cshtml` mistura html com cSharp, existem comandos como este que não existe por padrão no html:
```c#
@foreach (var item in Model)
{}
```
<br>

Esta parte do código nomeia a aba da pagina.
```c#
@{
    ViewData["Title"] = "Criar novo contato";
}
```

## Layout das paginas e cabeçalho
O MVC tem um arquivo que controla o layout padrão das paginas, ele fica em Views > Shared > _Layout.cshtml.
Nele você pode adicionar mais botões ao cabeçalho, modificar o rodapé, e alterar o tema da pagina alterando o arquivo bootstrap que ele chama para um arquivo que possua outras cores para os botões e backgrounds padrões do mvc.


<br>

## Enviando dados para as paginas

Esse método que fica na ContatoController, ele vai criar uma lista com todos os contatos que a context pegou do banco de dados, e retornar ela para a view Index.

![IndexContato](imgs/IndexContato.png)

Na view Index.cshtml a primeira linha de código, ela cria uma propriedade de lista IEnumerable, do tipo Contato, seguindo o caminho do projeto até a model contato, ai você pode utilizar a propriedade model para exibir o conteúdo recebido da context.

![Pagina-CSHTML](imgs/Pagina-CSHTML.png)

---

# Códigos/Tags



- `asp-action="NomeDeAlgumMetodoDaController"` --- Essa tag invoca um método lá da classe controller. Por exemplo, se ele estiver dentro de um botão e no parenteses tiver Index, ele vai chamar o método Index que vai chamar a pagina Index.

- `asp-for="Nome"` --- Esta tag indica qual a propriedade que está relacionado, por exemplo, se for usada em um campo de texto, ela vai identifica pra qual propriedade o valor inserido no campo vai ser enviado, e também trabalhar as necessidades desse campo, se a propriedade que vir do banco de dados estiver setado que não pode receber nulo, então vai tornar obrigatório que o campo não fique em branco antes de dar submit em um formulário por exemplo. E se for usado em um Label ele só da o nome da label igual a da propriedade.

- `class="control-label"` --- css? para formar o Label do campo de texto
- `class="form-control"` --- css? para formar o input do campo de texto.
- `class="btn btn-primary"` --- css? para formar um botão.
- `type="submit"` --- quando em um botão de conjunto de formulário, significa que é para submeter o formulário.
- `value="Criar"` --- quando em um botão, é o Texto do botão.

Exemplos:

```html
<div class="row">
    <div class="col-md-4">
        <!--Cria um formulário que irá retornar os dados para o método Criar-->
        <form asp-action="Criar">
            <!--Grupo do formulário referente ao rotulo e campo de entrada
            de texto da propriedade Nome-->
            <div class="form-group">
                <label asp-for="Nome" class="control-label"></label>
                <input asp-for="Nome" class="form-control" />
            </div>
            <!--Grupo do formulário referente ao rotulo e campo de entrada
            de texto da propriedade telefone-->
            <div class="form-group">
                <label asp-for="Telefone" class="control-label"></label>
                <input asp-for="Telefone" class="form-control" />
            </div>
            <!--Grupo do formulário referente ao rotulo a CHECKBOX da
            propriedade Ativo-->
            <div class="form-group">
                <label asp-for="Ativo" class="control-label"></label>
                <input type="checkbox" asp-for="Ativo" class="form-check-input" />
            </div>
            <br />
            <!--Grupo do formulário referente botão de submit que irá enviar
            os dados do formulário para o método setado na asp-action localizada
            na abertura da tag form-->
            <div class="form-group">
                <input type="submit" value="Criar" class="btn btn-primary" />
            </div>
        </form>
    </div>
</div>
<hr/>
<div>
    <!--Chama o método Index que irá chamar a View Index-->
    <a asp-action="Index">Voltar</a>
</div>
```