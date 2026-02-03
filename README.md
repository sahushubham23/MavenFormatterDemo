@ExtendWith(MockitoExtension.class)
class BselsurServiceImplTest {

    @InjectMocks
    private BselsurServiceImpl service;

    @Mock
    private Gartsurgr1Repository gartsurgr1Repository;

    @Mock
    private MessageService messageService;

    private BselsurParams bselsur;

    @BeforeEach
    void setUp() {
        bselsur = new BselsurParams();
        bselsur.setNdos(100L);
        bselsur.setCtypsur(1);
        bselsur.setNouvsur(2);
        bselsur.setCtypeve(3);
        bselsur.setNitv(10L);
        bselsur.setWcbie(5);
        bselsur.setFunctionname(BSELSUR_PR_OK);
    }

    /**
     * ✅ functionname = OK
     * ✅ count > 0
     */
    @Test
    void createSuretyDetails_success() {

        when(gartsurgr1Repository.pdokMenValidateItem(
                bselsur.getNdos(),
                bselsur.getCtypsur(),
                bselsur.getNouvsur(),
                bselsur.getCtypeve(),
                bselsur.getNitv(),
                bselsur.getWcbie()
        )).thenReturn(1);

        Integer result = service.createSuretyDetails(bselsur);

        assertEquals(1, result);

        verify(gartsurgr1Repository, times(1))
                .pdokMenValidateItem(
                        bselsur.getNdos(),
                        bselsur.getCtypsur(),
                        bselsur.getNouvsur(),
                        bselsur.getCtypeve(),
                        bselsur.getNitv(),
                        bselsur.getWcbie()
                );
    }

    /**
     * ❌ functionname = OK
     * ❌ count = 0 → exception
     */
    @Test
    void createSuretyDetails_countZero_throwsException() {

        when(gartsurgr1Repository.pdokMenValidateItem(
                bselsur.getNdos(),
                bselsur.getCtypsur(),
                bselsur.getNouvsur(),
                bselsur.getCtypeve(),
                bselsur.getNitv(),
                bselsur.getWcbie()
        )).thenReturn(0);

        when(messageService.stdSperror(
                any(), any(), anyInt(), isNull(), anyString(), any()
        )).thenReturn("ERR");

        assertThrows(
                CREClientException.class,
                () -> service.createSuretyDetails(bselsur)
        );
    }

    /**
     * 🔁 functionname ≠ OK
     * repository must NOT be called
     */
    @Test
    void createSuretyDetails_functionNotOk_skipsRepository() {

        bselsur.setFunctionname("NOT_OK");

        Integer result = service.createSuretyDetails(bselsur);

        assertNull(result);
        verifyNoInteractions(gartsurgr1Repository);
    }
}
