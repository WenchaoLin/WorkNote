`[soofree@DL785 bin]# ./pbsmrtpipe show-templates`

```
************************************
45 Registered Pipelines (name -> id)
************************************
  1. Example Dev 01 pipeline                                pbsmrtpipe.pipelines.dev_01
  2. Example Dev 01 Subread DataSet pipeline                pbsmrtpipe.pipelines.dev_01_ds
  3. Example Dev 01 crashing pipeline                       pbsmrtpipe.pipelines.dev_01_fail
  4. Example Dev 02 pipeline                                pbsmrtpipe.pipelines.dev_02
  5. Example Dev 03 pipelines                               pbsmrtpipe.pipelines.dev_03
  6. Example Dev 04                                         pbsmrtpipe.pipelines.dev_04
  7. Pipeline that leverages a python static tasks          pbsmrtpipe.pipelines.dev_04_w_static_task
  8. Reference Set Report                                   pbsmrtpipe.pipelines.dev_diagnostic
  9. Dev Hello Distributed Workflow Pipeline                pbsmrtpipe.pipelines.dev_dist
 10. Dev Local Hello Pipeline                               pbsmrtpipe.pipelines.dev_local
 11. Dev Local Hello Chunkable Pipeline                     pbsmrtpipe.pipelines.dev_local_chunk
 12. Dev local Task for Chunking pipelines                  pbsmrtpipe.pipelines.dev_local_fasta_chunk
 13. Base Modification Detection                            pbsmrtpipe.pipelines.ds_modification_detection
 14. Base Modification and Motif Analysis                   pbsmrtpipe.pipelines.ds_modification_motif_analysis
 15. XI- Experimental Assembly (HGAP 5) without reports     pbsmrtpipe.pipelines.hgap_cmd
 16. X - Experimental Assembly (HGAP 5) without reports     pbsmrtpipe.pipelines.hgap_lean
 17. Internal Consensus Read Mapping                        pbsmrtpipe.pipelines.pb_ccs_align
 18. Internal CCS Pipeline with SubreadSet Barcoding        pbsmrtpipe.pipelines.pb_ds_barcode_ccs
 19. Internal LAA Pipeline with SubreadSet Barcoding        pbsmrtpipe.pipelines.pb_ds_barcode_laa
 20. Internal IsoSeq pipeline                               pbsmrtpipe.pipelines.pb_isoseq
 21. Internal IsoSeq clustering pipeline                    pbsmrtpipe.pipelines.pb_isoseq_cluster
 22. PacBio Internal Modification Analysis                  pbsmrtpipe.pipelines.pb_modification_detection
 23. PacBio Internal Modification and Motif Analysis        pbsmrtpipe.pipelines.pb_modification_motif_analysis
 24. Falcon Pipeline                                        pbsmrtpipe.pipelines.pipe_falcon
 25. Falcon FOFN Pipeline                                   pbsmrtpipe.pipelines.pipe_falcon_with_fofn
 26. Polished Falcon Pipeline                               pbsmrtpipe.pipelines.polished_falcon
 27. Assembly (HGAP 4)                                      pbsmrtpipe.pipelines.polished_falcon_fat
 28. Assembly (HGAP 4) without reports                      pbsmrtpipe.pipelines.polished_falcon_lean
 29. RS movie Align                                         pbsmrtpipe.pipelines.sa3_align
 30. SubreadSet Mapping                                     pbsmrtpipe.pipelines.sa3_ds_align
 31. Unrolled Template SubreadSet Mapping                   pbsmrtpipe.pipelines.sa3_ds_align_unrolled
 32. Barcoding                                              pbsmrtpipe.pipelines.sa3_ds_barcode
 33. Circular Consensus Sequences (CCS 2)                   pbsmrtpipe.pipelines.sa3_ds_ccs
 34. CCS Mapping                                            pbsmrtpipe.pipelines.sa3_ds_ccs_align
 35. Genomic Consensus                                      pbsmrtpipe.pipelines.sa3_ds_genomic_consensus
 36. IsoSeq                                                 pbsmrtpipe.pipelines.sa3_ds_isoseq
 37. IsoSeq Classify Only                                   pbsmrtpipe.pipelines.sa3_ds_isoseq_classify
 38. Long Amplicon Analysis (LAA 2)                         pbsmrtpipe.pipelines.sa3_ds_laa
 39. Basic Resequencing                                     pbsmrtpipe.pipelines.sa3_ds_resequencing
 40. Resequencing                                           pbsmrtpipe.pipelines.sa3_ds_resequencing_fat
 41. Convert BAM to FASTX                                   pbsmrtpipe.pipelines.sa3_ds_subreads_to_fastx
 42. RS Movie to Subread DataSet                            pbsmrtpipe.pipelines.sa3_fetch
 43. Convert RS to BAM                                      pbsmrtpipe.pipelines.sa3_hdfsubread_to_subread
 44. RS movie Resequencing                                  pbsmrtpipe.pipelines.sa3_resequencing
 45. Site Acceptance Test (SAT)                             pbsmrtpipe.pipelines.sa3_sat
```
