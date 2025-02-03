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

    @Transactional
    public boolean deleteByRootId(Long rootId) {
        if (gdsDbMenuRepository.existsById(rootId)) {
            gdsDbMenuRepository.deleteById(rootId);
            return true;
        }
        return false;
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

    // Delete a record by rootId
    @DeleteMapping("/{rootId}")
    public ResponseEntity<String> deleteByRootId(@PathVariable Long rootId) {
        boolean isDeleted = gdsDbMenuService.deleteByRootId(rootId);
        if (isDeleted) {
            return ResponseEntity.ok("Record with rootId " + rootId + " deleted successfully.");
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}

DELETE /api/gdsdbmenu/1

import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;
import java.time.LocalDateTime;

@Entity
@Getter
@Setter
@Table(name = "gdsmenuchoice")
public class GdsMenuChoice {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Auto-generated if applicable
    @Column(name = "id", nullable = false, updatable = false)
    private Long id;

    @Column(name = "rootid", nullable = false)
    private Long rootId;

    @Column(name = "choice_name")
    private String choiceName;

    @Column(name = "choice_description")
    private String choiceDescription;

    @Column(name = "created_at")
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    // Add remaining columns as per your database schema
}


import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

public interface GdsMenuChoiceRepository extends JpaRepository<GdsMenuChoice, Long> {

    List<GdsMenuChoice> findByRootId(Long rootId);

    @Transactional
    void deleteByRootIdAndId(Long rootId, Long id);

    @Transactional
    void deleteByRootId(Long rootId);
}


import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service
public class GdsMenuChoiceService {

    private final GdsMenuChoiceRepository repository;

    public GdsMenuChoiceService(GdsMenuChoiceRepository repository) {
        this.repository = repository;
    }

    // Bulk insert or update
    @Transactional
    public List<GdsMenuChoice> saveOrUpdateChoices(List<GdsMenuChoice> choices) {
        return repository.saveAll(choices);
    }

    // Fetch by rootId
    public List<GdsMenuChoice> getChoicesByRootId(Long rootId) {
        return repository.findByRootId(rootId);
    }

    // Delete a specific record by rootId and id
    @Transactional
    public void deleteChoice(Long rootId, Long id) {
        repository.deleteByRootIdAndId(rootId, id);
    }

    // Delete all records by rootId
    @Transactional
    public void deleteChoicesByRootId(Long rootId) {
        repository.deleteByRootId(rootId);
    }
}


import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/gdsmenuchoice")
public class GdsMenuChoiceController {

    private final GdsMenuChoiceService service;

    public GdsMenuChoiceController(GdsMenuChoiceService service) {
        this.service = service;
    }

    // Bulk insert or update records
    @PostMapping("/save")
    public ResponseEntity<List<GdsMenuChoice>> saveOrUpdate(@RequestBody List<GdsMenuChoice> choices) {
        return ResponseEntity.ok(service.saveOrUpdateChoices(choices));
    }

    // Fetch records by rootId
    @GetMapping("/{rootId}")
    public ResponseEntity<List<GdsMenuChoice>> getByRootId(@PathVariable Long rootId) {
        List<GdsMenuChoice> choices = service.getChoicesByRootId(rootId);
        return choices.isEmpty() ? ResponseEntity.notFound().build() : ResponseEntity.ok(choices);
    }

    // Delete a single record by rootId and id
    @DeleteMapping("/{rootId}/{id}")
    public ResponseEntity<Void> deleteByRootIdAndId(@PathVariable Long rootId, @PathVariable Long id) {
        service.deleteChoice(rootId, id);
        return ResponseEntity.noContent().build();
    }

    // Delete all records by rootId
    @DeleteMapping("/{rootId}")
    public ResponseEntity<Void> deleteByRootId(@PathVariable Long rootId) {
        service.deleteChoicesByRootId(rootId);
        return ResponseEntity.noContent().build();
    }
}


DELETE /api/gdsmenuchoice/1/5

DELETE /api/gdsmenuchoice/1


