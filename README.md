import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

@Entity
@Getter
@Setter
@Table(name = "gdsdbmenu")
public class GdsDbMenu {

    @Id
    @Column(name = "rootid", nullable = false, updatable = false)
    private Long rootId;

    @Column(name = "column1")
    private String column1;

    @Column(name = "column2")
    private String column2;

    @Column(name = "column3")
    private String column3;

    @Column(name = "column4")
    private String column4;

    @Column(name = "column5")
    private String column5;
}


import org.springframework.data.jpa.repository.JpaRepository;

public interface GdsDbMenuRepository extends JpaRepository<GdsDbMenu, Long> {
}

import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.Optional;

@Service
public class GdsDbMenuService {

    private final GdsDbMenuRepository gdsDbMenuRepository;

    public GdsDbMenuService(GdsDbMenuRepository gdsDbMenuRepository) {
        this.gdsDbMenuRepository = gdsDbMenuRepository;
    }

    @Transactional
    public GdsDbMenu saveOrUpdateMenu(GdsDbMenu menu) {
        return gdsDbMenuRepository.save(menu);
    }

    public Optional<GdsDbMenu> getMenuByRootId(Long rootId) {
        return gdsDbMenuRepository.findById(rootId);
    }
}


import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.Optional;

@RestController
@RequestMapping("/api/gdsdbmenu")
public class GdsDbMenuController {

    private final GdsDbMenuService gdsDbMenuService;

    public GdsDbMenuController(GdsDbMenuService gdsDbMenuService) {
        this.gdsDbMenuService = gdsDbMenuService;
    }

    // Insert or update a record
    @PostMapping("/save")
    public ResponseEntity<GdsDbMenu> saveOrUpdate(@RequestBody GdsDbMenu menu) {
        GdsDbMenu savedMenu = gdsDbMenuService.saveOrUpdateMenu(menu);
        return ResponseEntity.ok(savedMenu);
    }

    // Fetch a record by rootId
    @GetMapping("/{rootId}")
    public ResponseEntity<GdsDbMenu> getByRootId(@PathVariable Long rootId) {
        Optional<GdsDbMenu> menu = gdsDbMenuService.getMenuByRootId(rootId);
        return menu.map(ResponseEntity::ok)
                   .orElseGet(() -> ResponseEntity.notFound().build());
    }
}

{
  "rootId": 1,
  "column1": "Value1",
  "column2": "Value2",
  "column3": "Value3",
  "column4": "Value4",
  "column5": "Value5"
}

