@ExtendWith(MockitoExtension.class)
class BselSurServiceImplTest {

    @InjectMocks
    private BselSurServiceImpl service;

    @Mock
    private GarSurgrnRepository garSurgrnRepository;

    @Mock
    private MessageService messageService;

    private BselSurParams params;

    @BeforeEach
    void setUp() {
        params = new BselSurParams();
        params.setNdos(100L);
        params.setCtypsur(1);
        params.setNouvsur(2);
        params.setCtypeve(3);
        params.setNitv(10L);
        params.setWcbie(5);
        params.setFunctionname("OK");
    }

    /**
     * ✅ Happy path
     * functionname = OK
     * count > 0
     */
    @Test
    void createSuretyDetails_success() {
        when(garSurgrnRepository.pdokMenValidateItem(
                anyLong(), anyInt(), anyInt(), anyInt(), anyLong(), anyInt()
        )).thenReturn(1);

        Integer result = service.createSuretyDetails(params);

        assertEquals(1, result);
        verify(garSurgrnRepository, times(1))
                .pdokMenValidateItem(
                        params.getNdos(),
                        params.getCtypsur(),
                        params.getNouvsur(),
                        params.getCtypeve(),
                        params.getNitv(),
                        params.getWcbie()
                );
    }

    /**
     * ❌ count == 0 → CREClientException
     */
    @Test
    void createSuretyDetails_whenCountZero_shouldThrowException() {
        when(garSurgrnRepository.pdokMenValidateItem(
                anyLong(), anyInt(), anyInt(), anyInt(), anyLong(), anyInt()
        )).thenReturn(0);

        when(messageService.stdSperror(
                any(), any(), any(), any(), any(), any()
        )).thenReturn("ERROR");

        assertThrows(
                CREClientException.class,
                () -> service.createSuretyDetails(params)
        );
    }

    /**
     * 🔁 functionname != OK
     * repository should NOT be called
     */
    @Test
    void createSuretyDetails_whenFunctionNotOk_shouldReturnNullOrZero() {
        params.setFunctionname("NOT_OK");

        Integer result = service.createSuretyDetails(params);

        assertNull(result); // change to assertEquals(0, result) if your method returns 0
        verifyNoInteractions(garSurgrnRepository);
    }
}
