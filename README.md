import jakarta.validation.Constraint;
import jakarta.validation.Payload;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Constraint(validatedBy = MapValuePatternValidator.class)
@Target({ ElementType.FIELD, ElementType.PARAMETER })
@Retention(RetentionPolicy.RUNTIME)
public @interface ValidMapValues {
    String regexp();
    String message() default "Invalid map value";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}


-------------------------

import jakarta.validation.ConstraintValidator;
import jakarta.validation.ConstraintValidatorContext;
import java.util.Map;
import java.util.regex.Pattern;

public class MapValuePatternValidator implements ConstraintValidator<ValidMapValues, Map<String, String>> {

    private Pattern pattern;

    @Override
    public void initialize(ValidMapValues constraintAnnotation) {
        this.pattern = Pattern.compile(constraintAnnotation.regexp());
    }

    @Override
    public boolean isValid(Map<String, String> map, ConstraintValidatorContext context) {
        if (map == null) {
            return true; // Consider null as valid; adjust if needed
        }

        for (Map.Entry<String, String> entry : map.entrySet()) {
            String value = entry.getValue();
            if (value != null && !pattern.matcher(value).matches()) {
                return false; // Fails if any value doesn’t match
            }
        }

        return true; // All values match the pattern
    }
}


-------------------------------

public class ApiRequestPayload {

    @ValidMapValues(regexp = "^[a-zA-Z0-9]*$", message = "Each value in variableMap must be alphanumeric")
    private Map<String, String> variableMap;

    private List<Level> level;

    // Getters and setters
}


-------------------

public class Level {

    @NotNull
    private Integer menu;

    @NotNull
    private Integer option;

    private Integer paraId1;
    private Integer paraId2;
    private Integer paraId3;

    @Valid // Ensure nested levels are also validated
    private List<Level> level;

    // Getters and setters
}


-------------------------


import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;
import jakarta.validation.Valid;

@RestController
public class ApiController {

    @PostMapping("/validatePayload")
    public String validatePayload(@Valid @RequestBody ApiRequestPayload payload) {
        // Process validated payload
        return "Validation passed!";
    }
}
