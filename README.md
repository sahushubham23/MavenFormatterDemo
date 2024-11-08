// Check for violations and handle the response
        if (!violations.isEmpty()) {
            StringBuilder errors = new StringBuilder("Validation errors:");
            for (ConstraintViolation<ApiRequestPayload> violation : violations) {
                errors.append(" ").append(violation.getPropertyPath()).append(": ").append(violation.getMessage());
            }
            return errors.toString();
        }
