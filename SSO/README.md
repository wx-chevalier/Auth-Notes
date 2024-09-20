# SSO

# 框架对比

| [ Keycloak](https://www.keycloak.org/) | [WSO2 Identity Server](https://wso2.com/identity-and-access-management) | [Gluu](https://www.gluu.org/)                                         | [CAS](https://apereo.github.io/cas/)               | [OpenAM](https://github.com/OpenIdentityPlatform/OpenAM/)                         | [Shibboleth IdP](https://www.shibboleth.net/products/identity-provider/)   |                                                                                |
| -------------------------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------- | -------------------------------------------------- | --------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| OpenID Connect/OAuth support           | yes                                                                     | yes                                                                   | yes                                                | yes                                                                               | yes                                                                        | **third-party**                                                                |
| Multi-factor authentication            | yes                                                                     | yes                                                                   | yes                                                | yes                                                                               | yes                                                                        | yes                                                                            |
| Admin UI                               | yes                                                                     | yes                                                                   | yes                                                | yes                                                                               | yes                                                                        | **no**                                                                         |
| OpenJDK support                        | yes                                                                     | [yes](https://docs.wso2.com/display/IS560/Installation+Prerequisites) |                                                    | [yes](https://apereo.github.io/cas/6.0.x/planning/Installation-Requirements.html) | [yes](https://backstage.forgerock.com/knowledge/kb/book/b16240196#openjdk) | [yes](https://wiki.shibboleth.net/confluence/display/IDP30/SystemRequirements) |
| Identity brokering                     | yes                                                                     | yes                                                                   | [yes](https://stackoverflow.com/a/54105614/399105) |                                                                                   |                                                                            |                                                                                |
| Middleware                             | Wildfly, JBOSS                                                          | **WSO2 Carbon**                                                       | Jetty, Apache HTTPD                                | any Java app server                                                               | any Java app server                                                        | Jetty, Tomcat                                                                  |
| Open source                            | yes                                                                     | **Note 1**                                                            | yes                                                | yes                                                                               | yes                                                                        | yes                                                                            |
| Commercial support                     | yes                                                                     | yes                                                                   | yes                                                | **third-party**                                                                   | yes                                                                        | **third-party**                                                                |
| Add federation metadata                | **no**                                                                  |                                                                       |                                                    |                                                                                   |                                                                            | yes                                                                            |
| Add metadata from URL                  | **no**                                                                  |                                                                       |                                                    |                                                                                   |                                                                            | yes                                                                            |
| Installation                           | easy                                                                    |                                                                       |                                                    |                                                                                   |                                                                            | **difficult**                                                                  |

# Links

- https://studygolang.com/articles/11794
- https://github.com/samitpal/simple-sso
- https://mp.weixin.qq.com/s/J6YJls05t2C4OGOqHVijhw