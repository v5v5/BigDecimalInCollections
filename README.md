    @Test
    @DisplayName(TEST_NAME + " с параметрами offset = 2, limit = 3")
    void withOffsetLimitTest() {
        int offset = 2;
        int limit = 3;
        List<RealCompetitionSummaryAmountDto> response = getSummaryAmountsRealCompetitions(
                Map.of("periods", currentPeriod, "onlyWithData", true, "offset", offset, "limit", limit), 200
        ).extract().jsonPath().getList(".", RealCompetitionSummaryAmountDto.class);
        assertThat(response).usingComparatorForType(BIG_DECIMAL_COMPARATOR, BigDecimal.class).hasSize(limit);
        assertThat(response)
                .usingRecursiveFieldByFieldElementComparator(
                        RecursiveComparisonConfiguration.builder()
                                .withComparatorForType(BigDecimal::compareTo, BigDecimal.class)
                                .build()
                ).isSubsetOf(expectedResult);
    }

        @Test
    @DisplayName(TEST_NAME + " с параметром realCompetitionCategory = пустая строка")
    void emptyRealCompetitionCategoryTest() {
        List<RealCompetitionSummaryAmountDto> response = getSummaryAmountsRealCompetitions(
                Map.of("periods", currentPeriod, "onlyWithData", true, "realCompetitionCategory", ""), 200
        ).extract().jsonPath().getList(".", RealCompetitionSummaryAmountDto.class);
        assertThat(response)
                .usingComparatorForType(BIG_DECIMAL_COMPARATOR, BigDecimal.class)
                .usingRecursiveComparison()
                .ignoringCollectionOrder()
                .isEqualTo(List.of());
    }

    https://stackoverflow.com/questions/63714635/assertj-fails-to-assert-bigdecimal-equality-without-scale
    см. 2-й ответ
