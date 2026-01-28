import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
class BseLsurServiceImplTest {

    @InjectMocks
    private BseLsurServiceImpl service;

    @Mock
    private GarSurgr1Repository garSurgr1Repository;

    // -------------------------
    // POSITIVE SCENARIOS
    // -------------------------

    @Test
    void shouldReturnCount_whenValidPbOkRequestAndRecordExists() {
        BseLsurParams params = buildValidParams();

        when(garSurgr1Repository.pbOkWhenValidateItem(
                params.getNdos(),
                params.getCtypsur(),
                params.getNouvsur(),
                params.getCtypeve()))
            .thenReturn(1);

        Integer result = service.createSuretyDetails(params);

        assertEquals(1, result);
        verify(garSurgr1Repository, times(1))
            .pbOkWhenValidateItem(100L, 1, 1, 1);
    }

    @Test
    void shouldReturnCount_whenMultipleRecordsFound() {
        BseLsurParams params = buildValidParams();

        when(garSurgr1Repository.pbOkWhenValidateItem(any(), any(), any(), any()))
            .thenReturn(3);

        Integer result = service.createSuretyDetails(params);

        assertEquals(3, result);
    }

    @Test
    void shouldReturnCount_whenCtypeveIsTwo() {
        BseLsurParams params = buildValidParams();
        params.setCtypeve(2);

        when(garSurgr1Repository.pbOkWhenValidateItem(any(), any(), any(), eq(2)))
            .thenReturn(1);

        Integer result = service.createSuretyDetails(params);

        assertEquals(1, result);
    }

    // -------------------------
    // NEGATIVE SCENARIOS
    // -------------------------

    @Test
    void shouldThrowException_whenNoRecordFound() {
        BseLsurParams params = buildValidParams();

        when(garSurgr1Repository.pbOkWhenValidateItem(any(), any(), any(), any()))
            .thenReturn(0);

        assertThrows(CREClientException.class,
            () -> service.createSuretyDetails(params));
    }

    @Test
    void shouldNotCallRepository_whenFunctionNameIsInvalid() {
        BseLsurParams params = buildValidParams();
        params.setFunctionname("INVALID_FUNCTION");

        Integer result = service.createSuretyDetails(params);

        assertNull(result);
        verifyNoInteractions(garSurgr1Repository);
    }

    @Test
    void shouldThrowNullPointerException_whenFunctionNameIsNull() {
        BseLsurParams params = buildValidParams();
        params.setFunctionname(null);

        assertThrows(NullPointerException.class,
            () -> service.createSuretyDetails(params));
    }

    @Test
    void shouldThrowException_whenNdosIsNull() {
        BseLsurParams params = buildValidParams();
        params.setNdos(null);

        assertThrows(Exception.class,
            () -> service.createSuretyDetails(params));
    }

    @Test
    void shouldThrowException_whenCtypsurIsNull() {
        BseLsurParams params = buildValidParams();
        params.setCtypsur(null);

        assertThrows(Exception.class,
            () -> service.createSuretyDetails(params));
    }

    @Test
    void shouldThrowException_whenNouvsurIsNull() {
        BseLsurParams params = buildValidParams();
        params.setNouvsur(null);

        assertThrows(Exception.class,
            () -> service.createSuretyDetails(params));
    }

    @Test
    void shouldThrowException_whenCtypeveIsNull() {
        BseLsurParams params = buildValidParams();
        params.setCtypeve(null);

        assertThrows(Exception.class,
            () -> service.createSuretyDetails(params));
    }

    // -------------------------
    // COMMON TEST DATA
    // -------------------------

    private BseLsurParams buildValidParams() {
        BseLsurParams params = new BseLsurParams();
        params.setFunctionname("BSELSUR_PB_OK");
        params.setNdos(100L);
        params.setCtypsur(1);
        params.setNouvsur(1);
        params.setCtypeve(1);
        params.setNitv(10L);
        params.setWcbie(1);
        return params;
    }
}
