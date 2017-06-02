Para a utilização de um componente do primefaces que exibe uma **lista** de **entidades** **selecionáveis** \(para a seleção de alunos e disciplinas para a matrícula, por exemplo\), precisamos implementar um **Converter** para **cada** **classe** envolvida.

Em nossa utilização do java em conjunto com o JSF, um converter é um componente que permite o **encapsulamento** de um ou mais objetos em uma **representação** via **texto** \(String\) - não sendo esta a visualização que o **usuário** terá, mas sim uma representação **interna** que permite por exemplo, saber qual dos objetos de uma lista foi **selecionada**.

A seguir temos os converters que utilizaremos em nosso projeto.

Eles estão descritos em um formato **base**, que pode ser modificado para funcionar com qualquer outra classe/entidade de qualquer outro projeto.

**AlunoConverter.java**

```java
package br.edu.utfpr.universidade;

import javax.faces.component.UIComponent;
import javax.faces.context.FacesContext;
import javax.faces.convert.Converter;
import javax.faces.convert.FacesConverter;

@FacesConverter(value = "alunoConverter")
public class AlunoConverter implements Converter {

    @Override
    public Object getAsObject(FacesContext facesContext, UIComponent uiComponent, String value) {
        if (value != null && !value.isEmpty()) {
            return (Aluno) uiComponent.getAttributes().get(value);
        }
        return null;
    }

    @Override
    public String getAsString(FacesContext facesContext, UIComponent uiComponent, Object value) {
        if (value instanceof Aluno) {
            Aluno entity = (Aluno) value;
            if (entity != null && entity instanceof Aluno && entity.getId() != null) {
                uiComponent.getAttributes().put(entity.getId().toString(), entity);
                return entity.getId().toString();
            }
        }
        return "";
    }
}
```

**DisciplinaConverter.java**

```java
package br.edu.utfpr.universidade;

import javax.faces.component.UIComponent;
import javax.faces.context.FacesContext;
import javax.faces.convert.Converter;
import javax.faces.convert.FacesConverter;

@FacesConverter(value = "disciplinaConverter")
public class DisciplinaConverter implements Converter {

    @Override
    public Object getAsObject(FacesContext facesContext, UIComponent uiComponent, String value) {
        if (value != null && !value.isEmpty()) {
            return (Disciplina) uiComponent.getAttributes().get(value);
        }
        return null;
    }

    @Override
    public String getAsString(FacesContext facesContext, UIComponent uiComponent, Object value) {
        if (value instanceof Disciplina) {
            Disciplina entity = (Disciplina) value;
            if (entity != null && entity instanceof Disciplina && entity.getId() != null) {
                uiComponent.getAttributes().put(entity.getId().toString(), entity);
                return entity.getId().toString();
            }
        }
        return "";
    }
}
```



