        replaceMenuAndOption(menuItem, replacementTable);
        addNewVariables(menuItem);

        // Convert MenuItem to JSON
        String modifiedJson = objectMapper.writeValueAsString(menuItem);
        System.out.println(modifiedJson);
    }

    private static void replaceMenuAndOption(MenuItem menuItem, Map<String, String> replacementTable) {
        // Replace menu and option values based on replacementTable
        String menu = menuItem.getMenu();
        String option = String.valueOf(menuItem.getOption());

        if (replacementTable.containsKey(menu)) {
            menuItem.setMenu(replacementTable.get(menu));
        }
        // You can do the same for the option if needed

        // Recursively iterate over the level
        if (menuItem.getLevel() != null) {
            for (MenuItem subMenuItem : menuItem.getLevel()) {
                replaceMenuAndOption(subMenuItem, replacementTable);
            }
        }
    }

    private static void addNewVariables(MenuItem menuItem) {
        // Add new variables as needed
        ((ObjectNode) menuItem).put("newVariable", "newValue");

        // Recursively iterate over the level
        if (menuItem.getLevel() != null) {
            for (MenuItem subMenuItem : menuItem.getLevel()) {
                addNewVariables(subMenuItem);
            }
        }
    }
