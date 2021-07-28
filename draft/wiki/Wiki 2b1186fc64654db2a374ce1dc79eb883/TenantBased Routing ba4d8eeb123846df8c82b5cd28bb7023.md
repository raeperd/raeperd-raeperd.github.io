# TenantBased Routing

Category: spring-boot
Created: December 15, 2020 5:58 PM
Created By: RaeCheol Park
Last Edited Time: December 15, 2020 5:58 PM
Status: NEED_DETAIL

```java
package com.seculetter.reporter.configuration;

import org.springframework.context.annotation.Configuration;
import org.springframework.data.mongodb.config.AbstractMongoClientConfiguration;
import org.springframework.security.core.context.SecurityContext;
import org.springframework.security.core.context.SecurityContextHolder;

import java.security.Principal;
import java.util.Optional;

@Configuration
public class DataSourceConfiguration extends AbstractMongoClientConfiguration {

    @Override
    protected String getDatabaseName() {
        return Optional.of(SecurityContextHolder.getContext())
                .map(SecurityContext::getAuthentication)
                .map(Principal::getName)
                .orElse("seculetter");
    }
}

```

```java
package com.seculetter.reporter.domain.tenant;

import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoDatabase;
import com.seculetter.reporter.service.TenantDetailsService;
import org.springframework.data.mongodb.core.SimpleMongoClientDatabaseFactory;
import org.springframework.security.core.userdetails.UserDetails;

public class MultiTenantDatabaseFactory extends SimpleMongoClientDatabaseFactory {

    private final TenantDetailsService tenantDetailsService;

    public MultiTenantDatabaseFactory(TenantDetailsService tenantDetailsService,
                                      MongoClient mongoClient, String databaseName) {
        super(mongoClient, databaseName);
        this.tenantDetailsService = tenantDetailsService;
    }

    @Override
    protected MongoDatabase doGetMongoDatabase(String dbName) {
        final UserDetails tenantDetails = tenantDetailsService.loadUserByUsername(dbName);
        return super.doGetMongoDatabase(tenantDetails.getUsername());
    }

}

```