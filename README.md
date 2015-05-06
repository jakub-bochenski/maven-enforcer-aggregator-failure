Given a company-pom and project structure:

       child (inherits from company-pom)
         +-- child2 (inherits from child)

When I run `mvn install` in child, with company-pom [configuration as in documentation][1], it fails with

    [INFO] --- maven-enforcer-plugin:1.4:enforce (enforce-distribution) @ child ---
    [DEBUG] Configuring mojo org.apache.maven.plugins:maven-enforcer-plugin:1.4:enforce from plugin realm ClassRealm[plugin>org.apache.maven.plugins:maven-enforcer-plugin:1.4, parent: sun.misc.Launcher$AppClassLoader@234f79cb]
    [DEBUG] Configuring mojo 'org.apache.maven.plugins:maven-enforcer-plugin:1.4:enforce' with basic configurator -->
    [DEBUG]   (s) fail = true
    [DEBUG]   (s) failFast = false
    [DEBUG]   (f) ignoreCache = false
    [DEBUG]   (f) mojoExecution = org.apache.maven.plugins:maven-enforcer-plugin:1.4:enforce {execution: enforce-distribution}
    [DEBUG]   (s) project = MavenProject: com.acme.maven:child:1-SNAPSHOT @ /home/egnyte/test/child/pom.xml
    [DEBUG]   (s) ignoreParent = true
    [DEBUG]   (s) rules = [org.apache.maven.plugins.enforcer.BanDistributionManagement@3020cc2d]
    [DEBUG]   (s) session = org.apache.maven.execution.MavenSession@4a0d851e
    [DEBUG]   (s) skip = false
    [DEBUG] -- end configuration --
    [DEBUG] Executing rule: org.apache.maven.plugins.enforcer.BanDistributionManagement
    [DEBUG] distributionManagement: org.apache.maven.model.DistributionManagement@393cb70a
    [DEBUG] Adding failure due to exception
    org.apache.maven.enforcer.rule.api.EnforcerRuleException: You have defined a repository in distributionManagement.
            at org.apache.maven.plugins.enforcer.utils.DistributionManagementCheck.execute(DistributionManagementCheck.java:46)
            at org.apache.maven.plugins.enforcer.BanDistributionManagement.execute(BanDistributionManagement.java:87)
            at org.apache.maven.plugins.enforcer.EnforceMojo.execute(EnforceMojo.java:150)
            at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:133)
            at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:208)
            at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:153)
            at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:145)
            at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:108)
            at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:76)
            at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:51)
            at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:116)
            at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:361)
            at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:155)
            at org.apache.maven.cli.MavenCli.execute(MavenCli.java:584)
            at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:213)
            at org.apache.maven.cli.MavenCli.main(MavenCli.java:157)
            at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
            at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
            at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
            at java.lang.reflect.Method.invoke(Method.java:606)
            at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:289)
            at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:229)
            at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:415)
            at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:356)
    [WARNING] Rule 0: org.apache.maven.plugins.enforcer.BanDistributionManagement failed with message:
    You have defined a repository in distributionManagement.


Whatever I do this rule seems to fail in a first child of a multi-module project.

Tried:

 - making child a module of company-pom (and running `mvn install` from company-pom directory)

 - explicitly setting `ignoreParent` to `true` and `false` 
 
 - direct-child `mvn install` is passing OK

  [1]: https://maven.apache.org/enforcer/enforcer-rules/banDistributionManagement.html
