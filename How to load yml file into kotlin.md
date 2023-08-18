
```kotlin
import com.fasterxml.jackson.databind.ObjectMapper  
import com.fasterxml.jackson.dataformat.yaml.YAMLFactory  
import com.fasterxml.jackson.module.kotlin.kotlinModule  
import ir.ebb.config.RepartitionerConfiguration  
import java.io.InputStream  
  
  
class PropertiesLoader(profile: String, app: String) {  
  
    private val mapper = ObjectMapper(YAMLFactory()).registerModule(kotlinModule())  
    private val ymlFileName = "application-$app-$profile.yml"  
  
    fun load(): RepartitionerConfiguration {  
        val src = findEnvironmentVariablesAndSetThem(  
            removeComments(javaClass.classLoader.getResourceAsStream(ymlFileName)  
                ?: throw RuntimeException("$ymlFileName wasn't found under resources"))  
        )  
        return mapper.readValue(src, RepartitionerConfiguration::class.java)  
    }  
  
    private fun findEnvironmentVariablesAndSetThem(src: String): String {  
        var envVarsSet = src  
        val envValueRegex = (Regex.escape("\${") + "(\\w*)" + 
	        Regex.escape("}")).toRegex()  
        for (m in envValueRegex.findAll(envVarsSet)) {  
            System.getenv(m.groupValues[1])?.let { envVar ->  
                envVarsSet = envVarsSet.replace(m.groupValues[0], envVar)  
            }  
        }  
        return envVarsSet  
    }  
  
    private fun removeComments(src: InputStream): String {  
        val regex = "#.*\\$\\{.*}\\n".toRegex()  
        var commentsCleared = String(src.readAllBytes())  
        for (found in regex.findAll(commentsCleared)) {  
            commentsCleared = commentsCleared.replace(found.groupValues[0], "\n")  
        }  
        return commentsCleared  
    }  
}
```