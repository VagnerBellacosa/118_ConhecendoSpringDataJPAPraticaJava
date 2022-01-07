# Melhorar desempenho JPA

Postado em: [23/01/2013](https://thiagovespa.com.br/blog/2013/01/23/melhorar-desempenho-jpa/) por: [Thiago Galbiatti Vespa](https://thiagovespa.com.br/blog/author/thiagovespa/) — [9 Comentários ↓](https://thiagovespa.com.br/blog/2013/01/23/melhorar-desempenho-jpa/#comments)



Abaixo temos algumas recomendações para melhorar o desempenho de aplicações com JPA e Hibernate

- Utilizar a annotation @BatchSize(size=TAMANHO) para melhorar o desempenho em entidades que recuperam vários registros.
- Utilizar *join fetch* ao invés de relacionamentos EAGER para coleções. Dessa forma evitará LazyInitializationException e carregar vários dados em memória com o EAGER.
- Utilizar EAGER em anotações @ManyToMany e @OneToMany somente em coleções que temos certeza que são pequenas, por exemplo com uns 5 registros no máximo.
- Caso queira evitar  consultas utilizando o *join fetch*, utilizar o utilitário Hibernate.initialize(objeto) para carregar a coleção LAZY quando necessário para não obter LazyInitializationException
- Evitar utilizar o Collections.sort e utilizar o order by do jpa.
- Evitar utilizar o ID nos métodos equals e hashCode, mais informações [nesse link](https://community.jboss.org/wiki/EqualsAndHashCode).
- Utilizar Named Queries para facilitar o gerenciamento.
- Utilizar setFirstResult, setMaxResults da classe Query em locais onde é desejado paginação ou retornam muitos resultados.
- Habilitar o cache de consultas no persistence.xml (em produção): <property name="hibernate.cache.use_query_cache">true</property>
- Desabilitar logs desnecessários no persistence.xml (em produção): <property name="hibernate.show_sql" value="false" /> e <property name="hibernate.format_sql" value="false" />

Exemplos de mapeamentos incorretos e lentos:

```
@ManyToMany``(fetch = FetchType.EAGER)``private` `Set algo;
```

Onde algo são vários registros que não são utilizados nas telas da aplicação. Outras possíveis formas de se melhorar o desempenho é utilizar cache secundário e habilitar cache de queries. Se você tiver alguma outra dica é só avisar.

[![facebook](https://thiagovespa.com.br/blog/wp-content/plugins/ultimate-social-media-icons/images/responsive-icon/facebook.svg) Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fthiagovespa.com.br%2Fblog%2F2013%2F01%2F23%2Fmelhorar-desempenho-jpa%2F)[![Twitter](https://thiagovespa.com.br/blog/wp-content/plugins/ultimate-social-media-icons/images/responsive-icon/Twitter.svg) Tweet](https://twitter.com/intent/tweet?text=Hey%2C+check+out+this+cool+site+I+found%3A+www.yourname.com+%23Topic+via%40my_twitter_name&url=https%3A%2F%2Fthiagovespa.com.br%2Fblog%2F2013%2F01%2F23%2Fmelhorar-desempenho-jpa%2F)[![Follow](data:image/webp;base64,UklGRo4DAABXRUJQVlA4TIEDAAAvlcAcEIegIG0Dpv5N74qY//kXCKRwg0tIAw+DByzbVqtIJL6uGg3zH25VtwIBVvdfRP8hSJLcttl1joiDOwD8AD16XgAAe4toAHMlgaf94vi+94lefr9xFm6PH+ycifH79eBcfPx+zcY/XjbOxytR6ZyPeyFwRj5Ses/MC+fkxv8/H5yTaeeMvBIt+fzRvhARlT0ddP8O6y7Ow/dMT0YqUd2jaw4Yb5KUYYv/P/tKIpcz/uySermjzy6Rg8MJpPR0Rc3z9Ea8/9k3Qd3nH+MoNpzOInkF75LSrD327Co741OgJTKcUIQAwo/Ok7B8jgGnaxL3A+QDvQRYkvtG8oUNT1AIB6ezauY3dADduveQ8tVgFojvUHB6AY4kuISyjG7j9mtWr7HQ92fxmH2B5KiRKQqcWlVgWawIcOobyZcwhQLA6SyKmayQfzjdizA00ksHJw9L8l6EnW8g33C6JkXIbSjui7O4vcsxZCArOPnDkFDm9NCU07wCa2crWcGp+cKQiUzh5AVDRrKFkwMMGcoaTuOjEkv5g5MCQwayyxVH1KrJkxRrDYCTuqgDE1wM5e90jUpS7OUITl2KIZfiyzDNOTQYGiA/cLqlGHIsvk3SnF0ICt/iXkhYV134RrWzc53Crdzymvk7SBvhZSt3am8YEn3JlRQDVTI6VixVVa4Cp7omGu3pcqgO8uCte1Or5MO1uVJfaYDLim2iYREhRmGo4P4OcU0rQRFDHU50zyq+tFkxocbGmxiCoXKoj8igXoxhoQEYWrs2qpwuB3kM7DFUmwgM2qjkXuzhBHMMPZVT+sWl+8iPYYyh+VI0nLaKfTWGE2wxtOsGWaGCbr3vcpjrnuVt6m6PylZ7ke8x+tvPg6GuydehzRg4QdjDjveu71ne0Fq16uvGwDA4QYIhP2ci9nBS6Cy+DplHwgk2g01k73CCdmL6PZe8tBEMRkCMOQCcoD4ccGTjCAbKxdGFB22t4Rcjgl476ZsYTlAcDgSwKs2BDEPgSBZOpEpRL8Fo0xzZ7Ap6k1DlAPeOzfaU07it2mq0S3sD95oG3IKOsiRb3xgPtCSncbv+9X/WRJ4QrC+zKw935HPX/znAzOgxkp0op4emzoP/fz75L/XcIt2ckTutnJFBFGJFGXJ6PXE+/hHgrRk9tZjRy/r48E8Clm1JrVn80/t9p2VCC/+9E79/ggAA) Follow us](https://follow.it/now)

[Google+](https://profiles.google.com/thiagogv?rel=author)

Este post foi publicado em: [Java](https://thiagovespa.com.br/blog/category/java/) Listada nas Tags: [base de dados](https://thiagovespa.com.br/blog/tag/base-de-dados-2/),[hibernate](https://thiagovespa.com.br/blog/tag/hibernate/),[java](https://thiagovespa.com.br/blog/tag/java-2/),[jpa](https://thiagovespa.com.br/blog/tag/jpa/) por: [Thiago Galbiatti Vespa](https://thiagovespa.com.br/blog/author/thiagovespa/). Arquivado em: [Link permanente](https://thiagovespa.com.br/blog/2013/01/23/melhorar-desempenho-jpa/).